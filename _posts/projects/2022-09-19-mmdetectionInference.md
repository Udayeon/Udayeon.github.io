---
layout: post
title: 
description: |
  Swin Transformer for Object Detection - Inference with COCO dataset
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Object Detection - Inference with COCO dataset
* * *


# 1. Reference
![image](https://user-images.githubusercontent.com/69246778/190968434-c13f7418-1785-4798-9c8a-4cb1f452244b.png)   
![image](https://user-images.githubusercontent.com/69246778/190968519-77c89add-13d1-43f1-b65f-58015b8e3161.png)   
[Cascade Mask R-CNN](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection#mask-r-cnn)

# 2. Configs gitclone
[git clone only some directories in git repository](https://infiduk.github.io/2022/02/09/git.html)      
[SwinTransformer/Swin-Transformer-Object-Detection/configs/swin](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection/tree/master/configs/swin)      
![image](https://user-images.githubusercontent.com/69246778/190962477-2dec06ad-3b27-4c84-b690-27ca1118b109.png)   

@ /mmdetection/mmdetection/configs/swin   
```
git init
git remote add origin https://github.com/SwinTransformer/Swin-Transformer-Object-Detection.git
git config core.sparsecheckout true
echo 'configs/swin/**' >> .git/info/sparse-checkout
git pull origin master
```   
![image](https://user-images.githubusercontent.com/69246778/190964172-e232732e-44d3-4593-8748-917697d38888.png)   
   
# 3. Base models gitclone
[SwinTransformer/Swin-Transformer-Object-Detection/configs/_base_/models](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection/tree/master/configs/_base_/models)   
![image](https://user-images.githubusercontent.com/69246778/190964904-f711c036-c94b-4407-9a7e-78f6262e2cc0.png)   
@ /mmdetection/mmdetection/configs/_base_/models   
```
git init
git remote add origin https://github.com/SwinTransformer/Swin-Transformer-Object-Detection.git
git config core.sparsecheckout true
echo 'configs/_base_/models/**' >> .git/info/sparse-checkout
git pull origin master
```
![image](https://user-images.githubusercontent.com/69246778/190965274-935cdfe9-5afa-44a1-85ee-5247b4ce9848.png)

# 4. Checkpoints 
![image](https://user-images.githubusercontent.com/69246778/190969580-217e9dc5-e0a9-46c0-a4d0-cf95d7aaf153.png)
Download the model and move them to @mmdetection/mmdetection/checkpoints   
![image](https://user-images.githubusercontent.com/69246778/191157829-8b7181fd-0e6e-4bb9-817c-f57abf65377d.png)


# 5. cascade_mask_rcnn_swin_tiny_patch4_window7

## 5.1. Inference  -  pretrained ImageNet-1k
[ImageNet-1K Pretrained Swin-V1 Models](https://github.com/microsoft/Swin-Transformer#main-results-on-imagenet-with-pretrained-models)
https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_tiny_patch4_window7_224.pth   
   
@ configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
```py
_base_ = [
    '../_base_/models/cascade_mask_rcnn_swin_fpn.py',
    '../_base_/datasets/coco_instance.py',
    '../_base_/schedules/schedule_1x.py', '../_base_/default_runtime.py'
]
pretrained = 'https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_tiny_patch4_window7_224.pth'
model = dict(
    type="CascadeRCNN",
    backbone=dict(
        embed_dim=96,
        depths=[2, 2, 6, 2],
        num_heads=[3, 6, 12, 24],
        window_size=7,
        ape=False,
        drop_path_rate=0.0,
        patch_norm=True,
        use_checkpoint=False,
        init_cfg=dict(type='Pretrained', checkpoint=pretrained)
        ),
```

@ configs/_base_/models/cascade_mask_rcnn_swin_fpn.py
```
# model settings
pretrained= 'https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_tiny_patch4_window7_224.pth'
model = dict(
    type='CascadeRCNN',
    backbone=dict(
        type='SwinTransformer',
        embed_dims=96,
        depths=[2, 2, 6, 2],
        num_heads=[3, 6, 12, 24],
        window_size=7,
        mlp_ratio=4.,
        qkv_bias=True,
        qk_scale=None,
        drop_rate=0.,
        attn_drop_rate=0.,
        drop_path_rate=0.2,
        patch_norm=True,
        out_indices=(0, 1, 2, 3),
        init_cfg=dict(type='Pretrained', checkpoint=pretrained)),
```

@ /mmdetection/mmdetection (Terminal)
```
# single-gpu testing
python tools/test.py configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py checkpoints/cascade_mask_rcnn_swin_tiny_patch4_window7.pth --eval bbox segm

# multi-gpu testing
tools/dist_test.sh configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py checkpoints/cascade_mask_rcnn_swin_tiny_patch4_window7.pth 1 --eval bbox segm
```
   
### 5.1.a. TypeError: CascadeRCNN: SwinTransformer: __init__() got an unexpected keyword argument 'embed_dim'
![image](https://user-images.githubusercontent.com/69246778/191161714-2cfe490e-3ec2-47ac-a847-3c9e044ec118.png)
@ configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
@ configs/_base_/models/cascade_mask_rcnn_swin_fpn.py   
```
embed_dim --> embed_dims
```

### 5.1.b. TypeError: CascadeRCNN: SwinTransformer: __init__() got an unexpected keyword argument 'ape'
![image](https://user-images.githubusercontent.com/69246778/191162135-05fc0b1b-f96a-44e3-998c-a5b8128f530d.png)   
'ape' is not in here   
@ mmdet/models/backbones/swin.py   
@ mmdet/models/detectors/cascade_rcnn.py   
   
```
@ configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
@ configs/_base_/models/cascade_mask_rcnn_swin_fpn.py   
delete 'ape'
```

### 5.1.c.TypeError: CascadeRCNN: SwinTransformer: __init__() got an unexpected keyword argument 'use_checkpoint'
![image](https://user-images.githubusercontent.com/69246778/191163054-1fc0a55d-6fd6-4433-b675-d8735c262b46.png)
'use_checkpoint' is not in here   
@ mmdet/models/backbones/swin.py   
@ mmdet/models/detectors/cascade_rcnn.py   
   
```
@ configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
@ configs/_base_/models/cascade_mask_rcnn_swin_fpn.py   
delete 'use_checkpoint'
```


### 5.1.d. TypeError: CascadeRCNN: SwinTransformer: empty() received an invalid combination of arguments - got (tuple, dtype=NoneType, device=NoneType), but expected one of:
![image](https://user-images.githubusercontent.com/69246778/191163510-a85ce7f2-0e6b-48a1-833a-7f68e3909399.png)
@ configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py
```py
pretrained = 'https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_tiny_patch4_window7_224.pth'  # noqa
model = dict(
    type="CascadeRCNN",
    backbone=dict(
        embed_dims=96,
        depths=[2, 2, 6, 2],
        num_heads=[3, 6, 12, 24],
        window_size=7,
        mlp_ratio=4,
        qkv_bias=True,
        qk_scale=None,
        drop_rate=0.,
        attn_drop_rate=0.,
        drop_path_rate=0.2,
        patch_norm=True,
        out_indices=(0, 1, 2, 3),
        with_cp=False,
        convert_weights=True,
        init_cfg=dict(type='Pretrained', checkpoint=pretrained)),

    neck=dict(in_channels=[96, 192, 384, 768]),
```

### 5.1.e. Results
![image](https://user-images.githubusercontent.com/69246778/191194365-f49c008a-6a66-4a52-8c44-e2c8fbdb959a.png)
Perhaps that has not been implemented. But, another example is been implemented.
```
python tools/test.py configs/swin/mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco.py checkpoints/mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco_20210906_131725-bacf6f7b.pth --eval bbox segm
```
   
**result**   
![image](https://user-images.githubusercontent.com/69246778/191197400-c6bb9a8a-ede1-4c2d-a1f5-57aff179d4bb.png)
![image](https://user-images.githubusercontent.com/69246778/191197432-eef663ad-c321-477e-8caf-3630d7e1051b.png)
![image](https://user-images.githubusercontent.com/69246778/191201695-8cea3008-3243-4190-b4bf-681936340db1.png)
**It is consistent with the Mask_R-CNN reference!**


### 5.1.f. 
@ configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
Retry using cascade_mask_rcnn_r50_fpn.py   
```py
_base_ = [
    '../_base_/models/cascade_mask_rcnn_r50_fpn.py',
    '../_base_/datasets/coco_instance.py',
    '../_base_/schedules/schedule_1x.py', '../_base_/default_runtime.py'
]
pretrained = 'https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_tiny_patch4_window7_224.pth'  # noqa
model = dict(
    type="CascadeRCNN",
    backbone=dict(
        _delete_=True,
        type='SwinTransformer',
        embed_dims=96,
        depths=[2, 2, 6, 2],
        num_heads=[3, 6, 12, 24],
        window_size=7,
        mlp_ratio=4,
        qkv_bias=True,
        qk_scale=None,
        drop_rate=0.,
        attn_drop_rate=0.,
        drop_path_rate=0.2,
        patch_norm=True,
        out_indices=(0, 1, 2, 3),
        with_cp=False,
        convert_weights=True,
        init_cfg=dict(type='Pretrained', checkpoint=pretrained)),

    neck=dict(in_channels=[96, 192, 384, 768]),
```
   
```py
python tools/test.py configs/swin/cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py checkpoints/cascade_mask_rcnn_swin_tiny_patch4_window7.pth --eval bbox segm
```
   
**Result**


