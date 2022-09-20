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
Download the model and move them to @mmdetection/mmdetection/checkpoints   
![image](https://user-images.githubusercontent.com/69246778/191157829-8b7181fd-0e6e-4bb9-817c-f57abf65377d.png)


# 5. cascade_mask_rcnn_swin_base_patch4_window7
## 5.1. Inference
@ /mmdetection/mmdetection   
```
# single-gpu testing
python tools/test.py configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py checkpoints/cascade_mask_rcnn_swin_base_patch4_window7.pth --eval bbox segm

# multi-gpu testing
tools/dist_test.sh configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py checkpoints/cascade_mask_rcnn_swin_base_patch4_window7.pth 1 --eval bbox segm
```
   
### 5.1.a. TypeError: CascadeRCNN: SwinTransformer: __init__() got an unexpected keyword argument 'embed_dim'
![image](https://user-images.githubusercontent.com/69246778/191161714-2cfe490e-3ec2-47ac-a847-3c9e044ec118.png)
@ configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
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
@ configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py   
@ configs/_base_/models/cascade_mask_rcnn_swin_fpn.py   
delete 'use_checkpoint'
```


### 5.1.d. TypeError: CascadeRCNN: SwinTransformer: empty() received an invalid combination of arguments - got (tuple, dtype=NoneType, device=NoneType), but expected one of:
![image](https://user-images.githubusercontent.com/69246778/191163510-a85ce7f2-0e6b-48a1-833a-7f68e3909399.png)

[Help](https://github.com/Udayeon/Udayeon.github.io/blob/main/_posts/projects/2022-09-07-TypeError:%20CascadeRCNN:%20SwinTransformer.md#typeerror-cascadercnn-swintransformer-empty-received-an-invalid-combination-of-arguments---got-tuple-dtypenonetype-devicenonetype)

### 5.1.e. Results
![image](https://user-images.githubusercontent.com/69246778/191181096-d8788cde-b17b-4921-9896-dc36c0238fd5.png)


## 5.2. Training
@ tools/test.py   
![image](https://user-images.githubusercontent.com/69246778/191160822-9de9d600-02b1-4648-9af2-56f6ebb567ff.png)   
![image](https://user-images.githubusercontent.com/69246778/191160944-2efedc2d-0e8f-40dc-ad51-90eddac192bf.png)   
**model** is build_detector from mmdet.models.   
   
@ mmdet/models/swin.py   
![image](https://user-images.githubusercontent.com/69246778/191159983-5028ed8e-2269-405b-8490-bb0d060cb771.png)   
![image](https://user-images.githubusercontent.com/69246778/191160684-370c8ba6-5dcc-4deb-b7f0-8eda1c0fb2b2.png)   
Keyword for using SwinTransformer pre-trained model is **SwinTransformer**     
   
To train a detector with pre-trained models, run:   
```
# single-gpu training
python tools/train.py configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py --cfg-options model.pretrained=SwinTransformer

# multi-gpu training
tools/dist_train.sh configs/swin/cascade_mask_rcnn_swin_base_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py 1 --cfg-options model.pretrained=SwinTransformer
```

## 5.2.a. KeyError: 'EpochBasedRunnerAmp is not in the runner registry'
![image](https://user-images.githubusercontent.com/69246778/191181531-16518f33-72f3-4351-8a57-71ba3e0fc539.png)
```

```
