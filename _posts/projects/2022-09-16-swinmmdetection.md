---
layout: post
title: 
description: |
  Swin Transformer for Object Detection - Demo Test
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Object Detection - Demo Test
* * *

# 1. Checkpoint
[README.md](https://github.com/open-mmlab/mmdetection/tree/master/configs/swin#swin)   
![image](https://user-images.githubusercontent.com/69246778/190587815-1f99d00f-6f8d-4e63-90da-719df04a7f23.png)   
Download configs and .pth file

# 2. In the Docker
@ /mmdetection/checkpoints   
![image](https://user-images.githubusercontent.com/69246778/190588591-0bc4b000-7f3e-4ea6-a2ab-68795d5a74ed.png)   

@ /mmdetection/configs/swin   
![image](https://user-images.githubusercontent.com/69246778/190588798-7732b9d3-8dc7-4e75-a792-345962b6175d.png)   

# 3. Tutorial
## 3.1. mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco
![image](https://user-images.githubusercontent.com/69246778/190961953-3109bdb1-a6cb-4830-9150-953d959666d6.png)

```
from mmdet.apis import init_detector, inference_detector
import mmcv

# Specify the path to model config and checkpoint file
config_file = 'configs/swin/mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco.py'
checkpoint_file = 'checkpoints/mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco_20210903_104808-b92c91f1.pth'

# build the model from a config file and a checkpoint file
model = init_detector(config_file, checkpoint_file, device='cuda:0')

# test a single image and show the results
img = 'demo/demo.jpg'  # or img = mmcv.imread(img), which will only load it once
result = inference_detector(model, img)
# visualize the results in a new window
model.show_result(img, result)
# or save the visualization results to image files
model.show_result(img, result, out_file='demo/result_mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco.jpg')

# test a video and show the results
video = mmcv.VideoReader('demo/demo.mp4')
for frame in video:
    result = inference_detector(model, frame)
    model.show_result(frame, result, wait_time=1)
```

## 3.2. mask_rcnn_swin-t-p4-w7_fpn_1x_coco
![image](https://user-images.githubusercontent.com/69246778/190962000-75614a16-11b2-4ea8-a364-1e3c06446155.png)
```
from mmdet.apis import init_detector, inference_detector
import mmcv

# Specify the path to model config and checkpoint file
config_file = 'configs/swin/mask_rcnn_swin-t-p4-w7_fpn_1x_coco.py'
checkpoint_file = 'checkpoints/mmask_rcnn_swin-t-p4-w7_fpn_1x_coco_20210902_120937-9d6b7cfa.pth'

# build the model from a config file and a checkpoint file
model = init_detector(config_file, checkpoint_file, device='cuda:0')

# test a single image and show the results
img = 'demo/demo.jpg'  # or img = mmcv.imread(img), which will only load it once
result = inference_detector(model, img)
# visualize the results in a new window
model.show_result(img, result)
# or save the visualization results to image files
model.show_result(img, result, out_file='demo/result_mask_rcnn_swin-t-p4-w7_fpn_1x_coco.jpg')

# test a video and show the results
video = mmcv.VideoReader('demo/demo.mp4')
for frame in video:
    result = inference_detector(model, frame)
    model.show_result(frame, result, wait_time=1)
```
## 3.3. mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco
![image](https://user-images.githubusercontent.com/69246778/190961896-dd3733a6-88f3-40ce-9930-1f06ddca2d65.png)
```
from mmdet.apis import init_detector, inference_detector
import mmcv

# Specify the path to model config and checkpoint file
config_file = 'configs/swin/mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco.py'
checkpoint_file = 'checkpoints/mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco_20210908_165006-90a4008c.pth'

# build the model from a config file and a checkpoint file
model = init_detector(config_file, checkpoint_file, device='cuda:0')

# test a single image and show the results
img = 'demo/demo.jpg'  # or img = mmcv.imread(img), which will only load it once
result = inference_detector(model, img)
# visualize the results in a new window
model.show_result(img, result)
# or save the visualization results to image files
model.show_result(img, result, out_file='demo/result_mask_rcnn_swin-t-p4-w7_fpn_fp16_ms-crop-3x_coco.jpg')

# test a video and show the results
video = mmcv.VideoReader('demo/demo.mp4')
for frame in video:
    result = inference_detector(model, frame)
    model.show_result(frame, result, wait_time=1)
```
## 3.4. mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco
![image](https://user-images.githubusercontent.com/69246778/190962048-f83ffdc9-d0d3-4efb-82d6-d6b97694e6cf.png)
```
from mmdet.apis import init_detector, inference_detector
import mmcv

# Specify the path to model config and checkpoint file
config_file = 'configs/swin/mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco.py'
checkpoint_file = 'checkpoints/mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco_20210906_131725-bacf6f7b.pth'

# build the model from a config file and a checkpoint file
model = init_detector(config_file, checkpoint_file, device='cuda:0')

# test a single image and show the results
img = 'demo/demo.jpg'  # or img = mmcv.imread(img), which will only load it once
result = inference_detector(model, img)
# visualize the results in a new window
model.show_result(img, result)
# or save the visualization results to image files
model.show_result(img, result, out_file='demo/result_mask_rcnn_swin-t-p4-w7_fpn_ms-crop-3x_coco.jpg')

# test a video and show the results
video = mmcv.VideoReader('demo/demo.mp4')
for frame in video:
    result = inference_detector(model, frame)
    model.show_result(frame, result, wait_time=1)
```


