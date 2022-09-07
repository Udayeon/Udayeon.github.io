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

# 1. Cloning repo
@ terminal
```
git clone https://github.com/open-mmlab/mmdetection.git
```

# 2. Install Docker
```
docker pull pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel
```
```
apt-get update && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```
```
pip install --no-cache-dir --upgrade pip wheel setuptools
RUN pip install mmcv-full==1.3.14 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html
```

```
conda clean --all
git clone https://github.com/open-mmlab/mmdetection.git /mmdetection
cd mmdetection
```

```
pip install --no-cache-dir -r requirements/build.txt
pip install --no-cache-dir -e .
```


RUN apt-get update && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install MMCV
RUN pip install mmcv-full==1.3.14 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html

# Install MMDetection
RUN conda clean --all
WORKDIR /workspace
ENV FORCE_CUDA="1"
```

@ Terminal(~/mmdetection/docker$)
```
docker build -t mmdetection .
```
