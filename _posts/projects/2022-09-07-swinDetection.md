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
{DATA_DRI} <-- directories that collect data in your environment.
```
docker run docker run --gpus all --shm-size=8g -it -v home:/mmdetection/data mmdetection
```

# 2.4. Using Docker containers in VS Code
@ vscode
```
a. ctrl + shift + p
b. remote-Containers:Attach to Running Container
c. choose container
```

# 2.4. Git clone in Container
@ New Terminal in VScode
```
git clone https://github.com/open-mmlab/mmdetection.git
```
mmdetection folder OPEN

@ New Terminal in VScode
![image](https://user-images.githubusercontent.com/69246778/189846600-db3ec60d-57a6-405b-9c0a-f35a1ea305fb.png)
```
pip install --no-cache-dir -r requirements/build.txt
pip install --no-cache-dir -e .
```
output   
![image](https://user-images.githubusercontent.com/69246778/189846880-a05cd7f4-d47e-4f48-8b11-5756fa1c3a14.png)
![image](https://user-images.githubusercontent.com/69246778/189847266-2e4d5020-ccc7-415c-be18-4168a1b34fe4.png)



# 3. Dataset Download
[github](https://github.com/open-mmlab/mmdetection/blob/master/docs/en/useful_tools.md#dataset-download)
@ terminal(cd mmdetection)
```
python tools/misc/download_dataset.py --dataset-name coco2017
```   
**output**   
![image](https://user-images.githubusercontent.com/69246778/189033022-88f63321-b237-454d-a41e-7dbc41eac763.png)   

**unzip install**   
[unzip](https://command-not-found.com/unzip)   
![image](https://user-images.githubusercontent.com/69246778/190091112-3f4897e9-df04-431f-b5a0-5b2765f64f57.png)   
```
apt-get update
apt-get install -y unzip
```
   
**unzip**   
@ cd rm /mmdetection/data/coco
```
unzip annotations_trainval2017.zip
unzip test2017.zip
unzip train2017.zip
unzip val2017.zip

rm annotations_trainval2017.zip
rm test2017.zip
rm train2017.zip
rm val2017.zip
```

![image](https://user-images.githubusercontent.com/69246778/190093255-98f8a407-a4de-48e2-93f1-4e3f3c3d99c9.png)


# 4. Tutorial
## 4.1. directory 
Create a **checkpoints** directory and get the next model file.
[faster_rcnn_r50_fpn_1x_coco checkpoint file](https://download.openmmlab.com/mmdetection/v2.0/faster_rcnn/faster_rcnn_r50_fpn_1x_coco/faster_rcnn_r50_fpn_1x_coco_20200130-047c8118.pth)

## 4.2. tutorial.py
Create a tutorial_1.py file and paste the following code.   
```
from mmdet.apis import init_detector, inference_detector
import mmcv

# Specify the path to model config and checkpoint file
config_file = 'configs/faster_rcnn/faster_rcnn_r50_fpn_1x_coco.py'
checkpoint_file = 'checkpoints/faster_rcnn_r50_fpn_1x_coco_20200130-047c8118.pth'

# build the model from a config file and a checkpoint file
model = init_detector(config_file, checkpoint_file, device='cuda:0')

# test a single image and show the results
img = 'demo/demo.jpg'  # or img = mmcv.imread(img), which will only load it once
result = inference_detector(model, img)
# visualize the results in a new window
model.show_result(img, result)
# or save the visualization results to image files
model.show_result(img, result, out_file='demo/demo_result.jpg')

# test a video and show the results
video = mmcv.VideoReader('demo/demo.mp4')
for frame in video:
    result = inference_detector(model, frame)
    model.show_result(frame, result, wait_time=1)
```

## 4.3. Result
/demo/demo_result.jpg 
