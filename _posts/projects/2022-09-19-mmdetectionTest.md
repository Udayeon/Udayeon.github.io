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

# 1. Configs gitclone
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
   
# 2. Base models gitclone
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

# 3. Checkpoints 



