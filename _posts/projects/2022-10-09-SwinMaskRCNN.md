---
layout: post
title: 
description: |
  Swin Transformer for Object Detection - Inference with COCO dataset 1. Mask RCNN (ResNet50)
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Object Detection - Inference with COCO dataset 1. Mask RCNN (ResNet50)
* * *

# 1. Reference
![image](https://user-images.githubusercontent.com/69246778/194818882-60f7ec72-2836-4a4a-ae67-b14813035f8d.png)   
[Results and Models Mask R-CNN](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection#mask-r-cnn)   

# 2. Checkpoints
[mmdetection Mask R-CNN download](https://github.com/open-mmlab/mmdetection/tree/master/configs/swin#mask-r-cnn)   
   
Make **checkpoints** directory at mmdetection.   

# 3. Inference

## 3.1. Swin Tiny Mask-RCNN
```
python tools/test.py configs/swin/mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco.py checkpoints/mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco_20210908_165006-90a4008c.pth --eval bbox segm
```   
   
**Results**   
![image](https://user-images.githubusercontent.com/69246778/194819673-4783ff62-9bcd-4680-a173-11d64857f375.png)   
![image](https://user-images.githubusercontent.com/69246778/194819680-77fa2dcc-dfd8-44be-871e-d744c76db8bb.png)   

## 3.2. Swin Small Mask-RCNN
```
python tools/test.py configs/swin/mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco.py checkpoints/mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco_20210903_104808-b92c91f1.pth --eval bbox segm
```
   
**ResultS**   
![image](https://user-images.githubusercontent.com/69246778/194819859-fd99791e-3901-4d47-b84f-2f3bfb333e98.png)   
![image](https://user-images.githubusercontent.com/69246778/194819879-e417f2f9-992a-4c42-a5c7-12c468fcd022.png)   

