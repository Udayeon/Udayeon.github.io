---
layout: post
title: 
description: |
  Swin Transformer for Object Detection 
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Object Detection 
* * *

# 1. Install Docker
```
docker pull pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel
```
```
docker run -it --gpus all --name detection pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel /bin/bash
```

@vscode
```
2.1. ctrl + shift + p
2.2. remote-Containers:Attach to Running Container
2.3. choose container
```

# 2. Build

@ terminal(vscode)
```
pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html
rm -rf mmdetection
git clone https://github.com/open-mmlab/mmdetection.git
cd mmdetection
pip install -e .
```

# 3. Dataset Download
[github](https://github.com/open-mmlab/mmdetection/blob/master/docs/en/useful_tools.md#dataset-download)
@ terminal(cd mmdetection)
```
python tools/misc/download_dataset.py --dataset-name coco2017
```   
**output**   
![image](https://user-images.githubusercontent.com/69246778/189033022-88f63321-b237-454d-a41e-7dbc41eac763.png)

