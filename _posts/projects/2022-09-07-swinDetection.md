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
# 1. Git Clone
```
git clone https://github.com/open-mmlab/mmdetection.git
```

# 2. Install Docker
[CUDA Version](https://mmdetection.readthedocs.io/en/latest/get_started.html#installation)
For Ampere-based NVIDIA GPUs, such as GeForce 30 series and NVIDIA A100, CUDA 11 is a must.   
Geforce 3070 requires **CUDA 11 or higher and cuDNN 8.0 or higher** because it has a compute capability of 8.6.   

```
cd mmdetection/docker
gedit Dockerfile
```
# 2.1. Dockerfile
```
ARG PYTORCH="1.9.0"   
ARG CUDA="11.1"       
ARG CUDNN="8"         

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="8.6"    
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu2004/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install MMCV
RUN pip install --no-cache-dir --upgrade pip wheel setuptools
RUN pip install --no-cache-dir mmcv-full==1.3.17 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html

# Install MMDetection
RUN conda clean --all
WORKDIR /mmdetection
ENV FORCE_CUDA="1"
RUN pip install --no-cache-dir -r requirements/build.txt
RUN pip install --no-cache-dir -e .
```

# 2.2. Build
@ /mmdetection/docker
```
docker build -t mmdetection .
```

# 2.3. Run
```
docker run --gpus all --shm-size=8g -it -v {DATA_DIR}:/mmdetection/data mmdetection
```

# 2.4. Using Docker containers in VS Code
@ vscode
```
a. ctrl + shift + p
b. remote-Containers:Attach to Running Container
c. choose container
```

# 3. Dataset Download
[github](https://github.com/open-mmlab/mmdetection/blob/master/docs/en/useful_tools.md#dataset-download)
@ terminal(cd mmdetection)
```
python tools/misc/download_dataset.py --dataset-name coco2017
```   
**output**   
![image](https://user-images.githubusercontent.com/69246778/189033022-88f63321-b237-454d-a41e-7dbc41eac763.png)

