---
layout: post
title: 
description: |
  BOAT CSwin-T Transformer for Classification - Inference with ImageNet1K(mini version) dataset
hide_image: true
tags:
  - projects
published: true
---

# BOAT CSwin-T Transformer for Classification - Inference with ImageNet1K(mini version) dataset
* * *

# 1. dataset
[ImageNet 1k Mini Ver](https://www.kaggle.com/datasets/ifigotin/imagenetmini-1000?resource=download)

# 2. Edit the swin_transformer.py
```py
# ------------------------------------------
# CSWin Transformer
# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.
# written By Xiaoyi Dong
# ------------------------------------------


import torch
import torch.nn as nn
import torch.nn.functional as F
from functools import partial

from timm.data import IMAGENET_DEFAULT_MEAN, IMAGENET_DEFAULT_STD
from timm.models.helpers import load_pretrained
from timm.models.layers import DropPath, to_2tuple, trunc_normal_
from timm.models.registry import register_model
from einops.layers.torch import Rearrange
import torch.utils.checkpoint as checkpoint
import numpy as np
import time
from einops import rearrange, repeat
import math



def _cfg(url='', **kwargs):
    return {
        'url': url,
        'num_classes': 1000, 'input_size': (3, 224, 224), 'pool_size': None,
        'crop_pct': .9, 'interpolation': 'bicubic',
        'mean': IMAGENET_DEFAULT_MEAN, 'std': IMAGENET_DEFAULT_STD,
        'first_conv': 'patch_embed.proj', 'classifier': 'head',
        **kwargs
    }


default_cfgs = {
    'cswin_224': _cfg(),
    'cswin_384': _cfg(
        crop_pct=1.0
    ),

}


class Mlp(nn.Module):
    def __init__(self, in_features, hidden_features=None, out_features=None, act_layer=nn.GELU, drop=0.):
        super().__init__()
        out_features = out_features or in_features
        hidden_features = hidden_features or in_features
        self.fc1 = nn.Linear(in_features, hidden_features)
        self.act = act_layer()
        self.fc2 = nn.Linear(hidden_features, out_features)
        self.drop = nn.Dropout(drop)

    def forward(self, x):
        x = self.fc1(x)
        x = self.act(x)
        x = self.drop(x)
        x = self.fc2(x)
        x = self.drop(x)
        return x

class LePEAttention(nn.Module):
    def __init__(self, dim, resolution, idx, split_size=7, dim_out=None, num_heads=8, attn_drop=0., proj_drop=0., qk_scale=None):
        super().__init__()
        self.dim = dim
        self.dim_out = dim_out or dim
        self.resolution = resolution
        self.split_size = split_size
        self.num_heads = num_heads
        head_dim = dim // num_heads
        # NOTE scale factor was wrong in my original version, can set manually to be compat with prev weights
        self.scale = qk_scale or head_dim ** -0.5
        if idx == -1:
            H_sp, W_sp = self.resolution, self.resolution
        elif idx == 0:
            H_sp, W_sp = self.resolution, self.split_size
        elif idx == 1:
            W_sp, H_sp = self.resolution, self.split_size
        else:
            print ("ERROR MODE", idx)
            exit(0)
        self.H_sp = H_sp
        self.W_sp = W_sp
        stride = 1
        self.get_v = nn.Conv2d(dim, dim, kernel_size=3, stride=1, padding=1,groups=dim)

        self.attn_drop = nn.Dropout(attn_drop)

    def im2cswin(self, x):
        B, N, C = x.shape
        H = W = int(np.sqrt(N))
        x = x.transpose(-2,-1).contiguous().view(B, C, H, W)
        x = img2windows(x, self.H_sp, self.W_sp)
        x = x.reshape(-1, self.H_sp* self.W_sp, self.num_heads, C // self.num_heads).permute(0, 2, 1, 3).contiguous()
        return x

    def get_lepe(self, x, func):
        B, N, C = x.shape
        H = W = int(np.sqrt(N))
        x = x.transpose(-2,-1).contiguous().view(B, C, H, W)

        H_sp, W_sp = self.H_sp, self.W_sp
        x = x.view(B, C, H // H_sp, H_sp, W // W_sp, W_sp)
        x = x.permute(0, 2, 4, 1, 3, 5).contiguous().reshape(-1, C, H_sp, W_sp) ### B', C, H', W'

        lepe = func(x) ### B', C, H', W'
        lepe = lepe.reshape(-1, self.num_heads, C // self.num_heads, H_sp * W_sp).permute(0, 1, 3, 2).contiguous()

        x = x.reshape(-1, self.num_heads, C // self.num_heads, self.H_sp* self.W_sp).permute(0, 1, 3, 2).contiguous()
        return x, lepe

    def forward(self, qkv):
        """
        x: B L C
        """
        q,k,v = qkv[0], qkv[1], qkv[2]

        ### Img2Window
        H = W = self.resolution
        B, L, C = q.shape
        assert L == H * W, "flatten img_tokens has wrong size"
        
        q = self.im2cswin(q)
        k = self.im2cswin(k)
        v, lepe = self.get_lepe(v, self.get_v)

        q = q * self.scale
        attn = (q @ k.transpose(-2, -1))  # B head N C @ B head C N --> B head N N
        attn = nn.functional.softmax(attn, dim=-1, dtype=attn.dtype)
        attn = self.attn_drop(attn)

        x = (attn @ v) + lepe
        x = x.transpose(1, 2).reshape(-1, self.H_sp* self.W_sp, C)  # B head N N @ B head N C

        ### Window2Img
        x = windows2img(x, self.H_sp, self.W_sp, H, W).view(B, -1, C)  # B H' W' C

        return x


class ContentAttention(nn.Module):
    def __init__(self, dim, window_size, num_heads, qkv_bias=True, qk_scale=None, attn_drop=0., proj_drop=0.):
        super().__init__()
        self.dim = dim
        self.window_size = window_size  # Wh, Ww
        self.ws = window_size
        self.num_heads = num_heads
        head_dim = dim // num_heads
        self.scale = qk_scale or head_dim ** -0.5
        self.qkv = nn.Linear(dim, dim * 3, bias=qkv_bias)
        self.attn_drop = nn.Dropout(attn_drop)
        self.proj = nn.Linear(dim, dim)
        self.proj_drop = nn.Dropout(proj_drop)
        self.softmax = nn.Softmax(dim=-1)
        self.get_v = nn.Conv2d(dim, dim, kernel_size=3, stride=1, padding=1,groups=dim)
    def forward(self, x, mask=None):
        #B_, W, H, C = x.shape
        #x = x.view(B_,W*H,C)
        B_, N, C = x.shape

        qkv = self.qkv(x).reshape(B_, N, 3, self.num_heads, C // self.num_heads).permute(2, 0, 3, 1, 4)# 3, B_, self.num_heads,N,D
        if True:
            q_pre = qkv[0].reshape(B_*self.num_heads,N, C // self.num_heads).permute(0,2,1)#qkv_pre[:,0].reshape(b*self.num_heads,qkvhd//3//self.num_heads,hh*ww)
            ntimes = int(math.log(N//49,2))
            q_idx_last = torch.arange(N).cuda().unsqueeze(0).expand(B_*self.num_heads,N)
            for i in range(ntimes):
                bh,d,n = q_pre.shape
                q_pre_new = q_pre.reshape(bh,d,2,n//2)
                q_avg = q_pre_new.mean(dim=-1)#.reshape(b*self.num_heads,qkvhd//3//self.num_heads,)
                q_avg = torch.nn.functional.normalize(q_avg,dim=-2)
                iters = 2
                for i in range(iters):
                    q_scores = torch.nn.functional.normalize(q_pre.permute(0,2,1),dim=-1).bmm(q_avg)
                    soft_assign = torch.nn.functional.softmax(q_scores*100, dim=-1).detach()
                    q_avg = q_pre.bmm(soft_assign)
                    q_avg = torch.nn.functional.normalize(q_avg,dim=-2)
                q_scores = torch.nn.functional.normalize(q_pre.permute(0,2,1),dim=-1).bmm(q_avg).reshape(bh,n,2)#.unsqueeze(2)
                q_idx = (q_scores[:,:,0]+1)/(q_scores[:,:,1]+1)
                _,q_idx = torch.sort(q_idx,dim=-1)
                q_idx_last = q_idx_last.gather(dim=-1,index=q_idx).reshape(bh*2,n//2)
                q_idx = q_idx.unsqueeze(1).expand(q_pre.size())
                q_pre = q_pre.gather(dim=-1,index=q_idx).reshape(bh,d,2,n//2).permute(0,2,1,3).reshape(bh*2,d,n//2)

            q_idx = q_idx_last.view(B_,self.num_heads,N)
            _,q_idx_rev = torch.sort(q_idx,dim=-1)
            q_idx = q_idx.unsqueeze(0).unsqueeze(4).expand(qkv.size())
            qkv_pre = qkv.gather(dim=-2,index=q_idx)
            q, k, v = rearrange(qkv_pre, 'qkv b h (nw ws) c -> qkv (b nw) h ws c', ws=49)

            k = k.view(B_*((N//49))//2,2,self.num_heads,49,-1)
            k_over1 = k[:,1,:,:20].unsqueeze(1)#.expand(-1,2,-1,-1,-1)
            k_over2 = k[:,0,:,29:].unsqueeze(1)#.expand(-1,2,-1,-1,-1)
            k_over = torch.cat([k_over1,k_over2],1)
            k = torch.cat([k,k_over],3).contiguous().view(B_*((N//49)),self.num_heads,49+20,-1)

            v = v.view(B_*((N//49))//2,2,self.num_heads,49,-1)
            v_over1 = v[:,1,:,:20].unsqueeze(1)#.expand(-1,2,-1,-1,-1)
            v_over2 = v[:,0,:,29:].unsqueeze(1)#.expand(-1,2,-1,-1,-1)
            v_over = torch.cat([v_over1,v_over2],1)
            v = torch.cat([v,v_over],3).contiguous().view(B_*((N//49)),self.num_heads,49+20,-1)

            #v = rearrange(v[:,:,:49,:], '(b nw) h ws d -> b h d (nw ws)', h=self.num_heads, b=B_)
            #W = int(math.sqrt(N))


        attn = (q @ k.transpose(-2, -1))*self.scale
        attn = self.softmax(attn)

        attn = self.attn_drop(attn)
        out = attn @ v
        
        out = rearrange(out, '(b nw) h ws d -> b (h d) nw ws', h=self.num_heads, b=B_)
        out = out.reshape(B_,self.num_heads,C//self.num_heads,-1)
        q_idx_rev = q_idx_rev.unsqueeze(2).expand(out.size())
        x = out.gather(dim=-1,index=q_idx_rev).reshape(B_,C,N).permute(0,2,1)


        v = rearrange(v[:,:,:49,:], '(b nw) h ws d -> b h d (nw ws)', h=self.num_heads, b=B_)
        W = int(math.sqrt(N))
        v = v.gather(dim=-1,index=q_idx_rev).reshape(B_,C,W,W)
        v = self.get_v(v)
        v = v.reshape(B_,C,N).permute(0,2,1)
        x = x + v



        x = self.proj(x)
        x = self.proj_drop(x)
        return x




class CSWinBlock(nn.Module):

    def __init__(self, dim, reso, num_heads,
                 split_size=7, mlp_ratio=4., qkv_bias=False, qk_scale=None,
                 drop=0., attn_drop=0., drop_path=0.,
                 act_layer=nn.GELU, norm_layer=nn.LayerNorm,
                 last_stage=False, content=False):
        super().__init__()
        self.dim = dim
        self.num_heads = num_heads
        self.patches_resolution = reso
        self.split_size = split_size
        self.mlp_ratio = mlp_ratio
        self.qkv = nn.Linear(dim, dim * 3, bias=qkv_bias)
        self.norm1 = norm_layer(dim)

        if self.patches_resolution == split_size:
            last_stage = True
        if last_stage:
            self.branch_num = 1
        else:
            self.branch_num = 2
        self.proj = nn.Linear(dim, dim)
        self.proj_drop = nn.Dropout(drop)
        self.content = content 
        if last_stage:
            self.attns = nn.ModuleList([
                LePEAttention(
                    dim, resolution=self.patches_resolution, idx = -1,
                    split_size=split_size, num_heads=num_heads, dim_out=dim,
                    qk_scale=qk_scale, attn_drop=attn_drop, proj_drop=drop)
                for i in range(self.branch_num)])
        else:
            self.attns = nn.ModuleList([
                LePEAttention(
                    dim//2, resolution=self.patches_resolution, idx = i,
                    split_size=split_size, num_heads=num_heads//2, dim_out=dim//2,
                    qk_scale=qk_scale, attn_drop=attn_drop, proj_drop=drop)
                for i in range(self.branch_num)])
        if self.content:
            self.content_attn =  ContentAttention(dim=dim, window_size=split_size, num_heads=num_heads, qkv_bias=qkv_bias, qk_scale=qkv_bias, attn_drop=attn_drop, proj_drop=attn_drop)
            self.norm3 = norm_layer(dim)
        mlp_hidden_dim = int(dim * mlp_ratio)

        self.drop_path = DropPath(drop_path) if drop_path > 0. else nn.Identity()
        self.mlp = Mlp(in_features=dim, hidden_features=mlp_hidden_dim, out_features=dim, act_layer=act_layer, drop=drop)
        self.norm2 = norm_layer(dim)

    def forward(self, x):
        """
        x: B, H*W, C
        """

        H = W = self.patches_resolution
        B, L, C = x.shape
        assert L == H * W, "flatten img_tokens has wrong size"
        img = self.norm1(x)
        qkv = self.qkv(img).reshape(B, -1, 3, C).permute(2, 0, 1, 3)
        
        if self.branch_num == 2:
            x1 = self.attns[0](qkv[:,:,:,:C//2])
            x2 = self.attns[1](qkv[:,:,:,C//2:])
            attened_x = torch.cat([x1,x2], dim=2)
        else:
            attened_x = self.attns[0](qkv)
        attened_x = self.proj(attened_x)
        x = x + self.drop_path(attened_x)
        if self.content:
            x = x + self.drop_path(self.content_attn(self.norm3(x)))

        x = x + self.drop_path(self.mlp(self.norm2(x)))

        return x

def img2windows(img, H_sp, W_sp):
    """
    img: B C H W
    """
    B, C, H, W = img.shape
    img_reshape = img.view(B, C, H // H_sp, H_sp, W // W_sp, W_sp)
    img_perm = img_reshape.permute(0, 2, 4, 3, 5, 1).contiguous().reshape(-1, H_sp* W_sp, C)
    return img_perm

def windows2img(img_splits_hw, H_sp, W_sp, H, W):
    """
    img_splits_hw: B' H W C
    """
    B = int(img_splits_hw.shape[0] / (H * W / H_sp / W_sp))

    img = img_splits_hw.view(B, H // H_sp, W // W_sp, H_sp, W_sp, -1)
    img = img.permute(0, 1, 3, 2, 4, 5).contiguous().view(B, H, W, -1)
    return img

class Merge_Block(nn.Module):
    def __init__(self, dim, dim_out, norm_layer=nn.LayerNorm):
        super().__init__()
        self.conv = nn.Conv2d(dim, dim_out, 3, 2, 1)
        self.norm = norm_layer(dim_out)

    def forward(self, x):
        B, new_HW, C = x.shape
        H = W = int(np.sqrt(new_HW))
        x = x.transpose(-2, -1).contiguous().view(B, C, H, W)
        x = self.conv(x)
        B, C = x.shape[:2]
        x = x.view(B, C, -1).transpose(-2, -1).contiguous()
        x = self.norm(x)
        
        return x

class SwinTransformer(nn.Module):
    """ Vision Transformer with support for patch or hybrid CNN input stage
    """
    def __init__(self, img_size=224, patch_size=4, in_chans=3, num_classes=1000, embed_dim=64, depths=[1,2,21,1], split_size = [1,2,7,7],
                 num_heads=[2,4,8,16], mlp_ratio=4., qkv_bias=True, qk_scale=None, drop_rate=0., attn_drop_rate=0.,
                 drop_path_rate=0., hybrid_backbone=None, norm_layer=nn.LayerNorm, use_checkpoint=False):
        super().__init__()
        self.use_checkpoint = use_checkpoint
        self.num_classes = num_classes
        self.num_features = self.embed_dim = embed_dim  # num_features for consistency with other models
        heads=num_heads

        self.stage1_conv_embed = nn.Sequential(
            nn.Conv2d(in_chans, embed_dim, 7, 4, 2),
            Rearrange('b c h w -> b (h w) c', h = img_size//4, w = img_size//4),
            nn.LayerNorm(embed_dim)
        )

        curr_dim = embed_dim
        dpr = [x.item() for x in torch.linspace(0, drop_path_rate, np.sum(depths))]  # stochastic depth decay rule
        self.stage1 = nn.ModuleList([
            CSWinBlock(
                dim=curr_dim, num_heads=heads[0], reso=img_size//4, mlp_ratio=mlp_ratio,
                qkv_bias=qkv_bias, qk_scale=qk_scale, split_size=split_size[0],
                drop=drop_rate, attn_drop=attn_drop_rate,
                drop_path=dpr[i], norm_layer=norm_layer, content=(i%2==0))
            for i in range(depths[0])])

        self.merge1 = Merge_Block(curr_dim, curr_dim*2)
        curr_dim = curr_dim*2
        self.stage2 = nn.ModuleList(
            [CSWinBlock(
                dim=curr_dim, num_heads=heads[1], reso=img_size//8, mlp_ratio=mlp_ratio,
                qkv_bias=qkv_bias, qk_scale=qk_scale, split_size=split_size[1],
                drop=drop_rate, attn_drop=attn_drop_rate,
                drop_path=dpr[np.sum(depths[:1])+i], norm_layer=norm_layer,content=(i%2==0))
            for i in range(depths[1])])
        
        self.merge2 = Merge_Block(curr_dim, curr_dim*2)
        curr_dim = curr_dim*2
        temp_stage3 = []
        temp_stage3.extend(
            [CSWinBlock(
                dim=curr_dim, num_heads=heads[2], reso=img_size//16, mlp_ratio=mlp_ratio,
                qkv_bias=qkv_bias, qk_scale=qk_scale, split_size=split_size[2],
                drop=drop_rate, attn_drop=attn_drop_rate,
                drop_path=dpr[np.sum(depths[:2])+i], norm_layer=norm_layer,content=(i%2==0))
            for i in range(depths[2])])

        self.stage3 = nn.ModuleList(temp_stage3)
        
        self.merge3 = Merge_Block(curr_dim, curr_dim*2)
        curr_dim = curr_dim*2
        self.stage4 = nn.ModuleList(
            [CSWinBlock(
                dim=curr_dim, num_heads=heads[3], reso=img_size//32, mlp_ratio=mlp_ratio,
                qkv_bias=qkv_bias, qk_scale=qk_scale, split_size=split_size[-1],
                drop=drop_rate, attn_drop=attn_drop_rate,
                drop_path=dpr[np.sum(depths[:-1])+i], norm_layer=norm_layer, last_stage=True,content=(i%2==0))
            for i in range(depths[-1])])
       
        self.norm = norm_layer(curr_dim)
        # Classifier head
        self.head = nn.Linear(curr_dim, num_classes) if num_classes > 0 else nn.Identity()

        trunc_normal_(self.head.weight, std=0.02)
        self.apply(self._init_weights)
    def _init_weights(self, m):
        if isinstance(m, nn.Linear):
            trunc_normal_(m.weight, std=.02)
            if isinstance(m, nn.Linear) and m.bias is not None:
                nn.init.constant_(m.bias, 0)
        elif isinstance(m, (nn.LayerNorm, nn.BatchNorm2d)):
            nn.init.constant_(m.bias, 0)
            nn.init.constant_(m.weight, 1.0)

    @torch.jit.ignore
    def no_weight_decay(self):
        return {'pos_embed', 'cls_token'}

    def get_classifier(self):
        return self.head
    
    def reset_classifier(self, num_classes, global_pool=''):
        if self.num_classes != num_classes:
            print ('reset head to', num_classes)
            self.num_classes = num_classes
            self.head = nn.Linear(self.out_dim, num_classes) if num_classes > 0 else nn.Identity()
            self.head = self.head.cuda()
            trunc_normal_(self.head.weight, std=.02)
            if self.head.bias is not None:
                nn.init.constant_(self.head.bias, 0)

    def forward_features(self, x):
        B = x.shape[0]
        x = self.stage1_conv_embed(x)
        for blk in self.stage1:
            if self.use_checkpoint:
                x = checkpoint.checkpoint(blk, x)
            else:
                x = blk(x)
        for pre, blocks in zip([self.merge1, self.merge2, self.merge3], 
                               [self.stage2, self.stage3, self.stage4]):
            x = pre(x)
            for blk in blocks:
                if self.use_checkpoint:
                    x = checkpoint.checkpoint(blk, x)
                else:
                    x = blk(x)
        x = self.norm(x)
        return torch.mean(x, dim=1)

    def forward(self, x):
        x = self.forward_features(x)
        x = self.head(x)
        return x
```
# 3. Edit the siwn_tiny_patch4_window7_224.yaml
```py
MODEL:
  TYPE: swin
  NAME: swin_tiny_patch4_window7_224
  DROP_PATH_RATE: 0.3
  SWIN:
    EMBED_DIM: 64
    DEPTHS: [ 1, 2, 21, 1 ]
    NUM_HEADS: [ 2, 4, 8, 16 ]

```

* **Need to edit a batch size and the number of GPU**
# 4. Training
```
python -m torch.distributed.launch --nproc_per_node 4 --master_port 12345  main.py --cfg configs/swin/swin_tiny_patch4_window7_224.yaml --data-path imagenet --batch-size 54
```

# 5. Evaluation
```
python -m torch.distributed.launch --nproc_per_node 2 --master_port 12345 main.py --eval --cfg configs/swin/swin_tiny_patch4_window7_224.yaml --resume checkpoints/BOATCSwin.pth --data-path imagenet
```
   
![image](https://user-images.githubusercontent.com/69246778/219988873-f023811d-c0c4-4a2e-9561-88e72d927561.png)

