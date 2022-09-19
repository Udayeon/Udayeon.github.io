---
layout: post
title: 
description: |
  Swin Transformer for Object Detection - Test COCO dataset
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Object Detection - Test COCO dataset
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
![image](https://user-images.githubusercontent.com/69246778/190970472-01631ec6-71f6-4564-919b-a355f0e3ed61.png)   
Download the model and move them to @mmdetection/mmdetection/checkpoints   
   
![image](https://user-images.githubusercontent.com/69246778/190970569-cdb5a9c3-6904-4c2a-8a95-49e837384518.png)

# 5. Test
# 5.1. cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco
cascade_mask_rcnn_swin_tiny_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py
```
_base_ = [
    '../_base_/models/cascade_mask_rcnn_swin_fpn.py',
    '../_base_/datasets/coco_instance.py',
    '../_base_/schedules/schedule_1x.py', '../_base_/default_runtime.py'
]

model = dict(
    backbone=dict(
        embed_dims=96,  # embed_dim --> embed_dims
        depths=[2, 2, 6, 2],
        num_heads=[3, 6, 12, 24],
        window_size=7,
        ape=False,
        drop_path_rate=0.2,
        patch_norm=True,
        use_checkpoint=False
    ),
    neck=dict(in_channels=[96, 192, 384, 768]),
    roi_head=dict(
        bbox_head=[
            dict(
                type='ConvFCBBoxHead',
                num_shared_convs=4,
                num_shared_fcs=1,
                in_channels=256,
                conv_out_channels=256,
                fc_out_channels=1024,
                roi_feat_size=7,
                num_classes=80,
                bbox_coder=dict(
                    type='DeltaXYWHBBoxCoder',
                    target_means=[0., 0., 0., 0.],
                    target_stds=[0.1, 0.1, 0.2, 0.2]),
                reg_class_agnostic=False,
                reg_decoded_bbox=True,
                norm_cfg=dict(type='SyncBN', requires_grad=True),
                loss_cls=dict(
                    type='CrossEntropyLoss', use_sigmoid=False, loss_weight=1.0),
                loss_bbox=dict(type='GIoULoss', loss_weight=10.0)),
            dict(
                type='ConvFCBBoxHead',
                num_shared_convs=4,
                num_shared_fcs=1,
                in_channels=256,
                conv_out_channels=256,
                fc_out_channels=1024,
                roi_feat_size=7,
                num_classes=80,
                bbox_coder=dict(
                    type='DeltaXYWHBBoxCoder',
                    target_means=[0., 0., 0., 0.],
                    target_stds=[0.05, 0.05, 0.1, 0.1]),
                reg_class_agnostic=False,
                reg_decoded_bbox=True,
                norm_cfg=dict(type='SyncBN', requires_grad=True),
                loss_cls=dict(
                    type='CrossEntropyLoss', use_sigmoid=False, loss_weight=1.0),
                loss_bbox=dict(type='GIoULoss', loss_weight=10.0)),
            dict(
                type='ConvFCBBoxHead',
                num_shared_convs=4,
                num_shared_fcs=1,
                in_channels=256,
                conv_out_channels=256,
                fc_out_channels=1024,
                roi_feat_size=7,
                num_classes=80,
                bbox_coder=dict(
                    type='DeltaXYWHBBoxCoder',
                    target_means=[0., 0., 0., 0.],
                    target_stds=[0.033, 0.033, 0.067, 0.067]),
                reg_class_agnostic=False,
                reg_decoded_bbox=True,
                norm_cfg=dict(type='SyncBN', requires_grad=True),
                loss_cls=dict(
                    type='CrossEntropyLoss', use_sigmoid=False, loss_weight=1.0),
                loss_bbox=dict(type='GIoULoss', loss_weight=10.0))
        ]))

img_norm_cfg = dict(
    mean=[123.675, 116.28, 103.53], std=[58.395, 57.12, 57.375], to_rgb=True)

# augmentation strategy originates from DETR / Sparse RCNN
train_pipeline = [
    dict(type='LoadImageFromFile'),
    dict(type='LoadAnnotations', with_bbox=True, with_mask=True),
    dict(type='RandomFlip', flip_ratio=0.5),
    dict(type='AutoAugment',
         policies=[
             [
                 dict(type='Resize',
                      img_scale=[(480, 1333), (512, 1333), (544, 1333), (576, 1333),
                                 (608, 1333), (640, 1333), (672, 1333), (704, 1333),
                                 (736, 1333), (768, 1333), (800, 1333)],
                      multiscale_mode='value',
                      keep_ratio=True)
             ],
             [
                 dict(type='Resize',
                      img_scale=[(400, 1333), (500, 1333), (600, 1333)],
                      multiscale_mode='value',
                      keep_ratio=True),
                 dict(type='RandomCrop',
                      crop_type='absolute_range',
                      crop_size=(384, 600),
                      allow_negative_crop=True),
                 dict(type='Resize',
                      img_scale=[(480, 1333), (512, 1333), (544, 1333),
                                 (576, 1333), (608, 1333), (640, 1333),
                                 (672, 1333), (704, 1333), (736, 1333),
                                 (768, 1333), (800, 1333)],
                      multiscale_mode='value',
                      override=True,
                      keep_ratio=True)
             ]
         ]),
    dict(type='Normalize', **img_norm_cfg),
    dict(type='Pad', size_divisor=32),
    dict(type='DefaultFormatBundle'),
    dict(type='Collect', keys=['img', 'gt_bboxes', 'gt_labels', 'gt_masks']),
]
data = dict(train=dict(pipeline=train_pipeline))

optimizer = dict(_delete_=True, type='AdamW', lr=0.0001, betas=(0.9, 0.999), weight_decay=0.05,
                 paramwise_cfg=dict(custom_keys={'absolute_pos_embed': dict(decay_mult=0.),
                                                 'relative_position_bias_table': dict(decay_mult=0.),
                                                 'norm': dict(decay_mult=0.)}))
lr_config = dict(step=[27, 33])
runner = dict(type='EpochBasedRunnerAmp', max_epochs=36)

# do not use mmdet version fp16
fp16 = None
optimizer_config = dict(
    type="DistOptimizerHook",
    update_interval=1,
    grad_clip=None,
    coalesce=True,
    bucket_size_mb=-1,
    use_fp16=True,
)
```

@ Tutorial.py
[demo](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection/blob/master/demo/image_demo.py)
```
from argparse import ArgumentParser
from mmdet.apis import inference_detector, init_detector, show_result_pyplot

def main():
    parser = ArgumentParser()
    parser.add_argument('img', help='Image file')
    parser.add_argument('config', help='Config file')
    parser.add_argument('checkpoint', help='Checkpoint file')
    parser.add_argument(
        '--device', default='cuda:0', help='Device used for inference')
    parser.add_argument(
        '--score-thr', type=float, default=0.3, help='bbox score threshold')
    args = parser.parse_args()

    # build the model from a config file and a checkpoint file
    model = init_detector(args.config, args.checkpoint, device=args.device)
    # test a single image
    result = inference_detector(model, args.img)
    # show the results
    show_result_pyplot(model, args.img, result, score_thr=args.score_thr)


if __name__ == '__main__':
    main()
```
   
error
![image](https://user-images.githubusercontent.com/69246778/190973969-fcf4d9aa-2a4a-44d4-b0f2-78102d35f1bc.png)
