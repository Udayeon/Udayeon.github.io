---
layout: post
title: 
description: |
  Swin Transformer
hide_image: true
tags:
  - deeplearning
published: true
---

# (Python) Swin Transformer 
* * *
[링크](https://github.com/microsoft/Swin-Transformer/blob/main/models/swin_transformer.py)
[Github](https://github.com/microsoft/Swin-Transformer)

# 1. README.md
![image](https://user-images.githubusercontent.com/69246778/177745316-6af7f2c0-ede7-4d7a-8e4c-0a4b0afeb5a3.png)
**Image classificaiton**, **Object Detection and Instance Segmentation**,  **Semantic Segmentation** 이 세 가지 task를
수행할 예정이다. 

## 1.1. Introduction
![image](https://user-images.githubusercontent.com/69246778/177745833-3da91bb6-577c-4b26-8772-e93fafb03fad.png)
아래 쪽에 내려보면 **Swin Transformer**에 대한 간략한 설명이 있다. 해석해보면, 일단 이 Swin transformer가 컴퓨터 비전을 위한 **Backbone**이라는 것. 
그리고, **계층적 구조와 shifted window라는 기법**을 사용한다는 것이 특징이다. (shifted window에 대한 설명은 논문 리뷰로 다룰 거임)   
**COCO object detection**에서 test-dev에 대해 **box AP 58.7** & **51.1 mask AP**을 달성,   
**ADE20K semantic segmentation**에서 val에 대해 **53.5 mIoU**를 달성함. 

## 1.3. Main Results on ImageNet with Pretrained Models & Main Results on Downstream Tasks
ImageNet으로 pretrain된 모델들의 주요 결과들을 제시함. 
[Results](https://github.com/microsoft/Swin-Transformer#main-results-on-imagenet-with-pretrained-models)

# 2. Image Classification
## 2.1. get_started.md
![image](https://user-images.githubusercontent.com/69246778/177749047-1a2905ef-df16-45f5-a3f5-ac643014c3ab.png)
연결된 MODEL HUB로 들어가서 사전 훈련된 모델들의 모음을 구경할 수 있음.     

![image](https://user-images.githubusercontent.com/69246778/177750465-a76b6bce-254b-4c51-abde-4577fcf9ebb4.png)
나는 siwn-V1의 swin-T를 사용하려 한다. ImageNet-1K로 사전훈련된 모델이다. 걸려진 링크의 **log**에서 훈련 과정 기록도
볼 수 있음.

## 2.2. Install
![image](https://user-images.githubusercontent.com/69246778/177751205-7b97fdc5-a80f-47b5-b3db-8663fe02d595.png)
설치조건을 살펴보자   
* python version = 3.7
* CUDA >= 10.2  (with cudnn >= 7)
* PyTorch >= 1.8.0 and torchvision >= 0.9.8 (with CUDA >= 10.2)

### 2.2.1. pytorch docker
각 프로젝트마다 Python과 라이브러리들의 버전이 다른 경우 가상환경(python venv나 conda 등)을 사용하면 
각 프로젝트의 환경을 분리해서 python의 여러 버전을 다룰 수 있다. 이때 사용하는 것이 Docker.

### 2.2.2. Clone
docker를 사용하면 좋지만 colab에서 시도해봄.   
게시물 [Colab에서 github 코드 사용하기](https://udayeon.github.io/2022/07/07/gitClone/) 참조.
![image](https://user-images.githubusercontent.com/69246778/177752506-c4fd2bcc-6a07-422b-975c-41fdedd13935.png)
SwinTransformer 폴더 안에 github을 clone해 놓음.

### 2.2.3. 각종 version 확인
```py
import torch
import torchvision

!python --version
print("Torch version:{}".format(torch.__version__))
print("cuda version: {}".format(torch.version.cuda))
print("cudnn version:{}".format(torch.backends.cudnn.version()))
print("Torchvision version:{}".format(torchvision.__version__))
```
![image](https://user-images.githubusercontent.com/69246778/177754942-10b9bcc6-65b3-4fe0-8bba-00fe95442ea6.png)
pytorch만 1.8이상으로 업데이트 해주면 되는 상황.   
   
```py
pip install timm==0.4.12
pip install opencv-python==4.4.0.46 termcolor==1.1.0 yacs==0.1.8
```
이렇게 설치를 다하면   
![image](https://user-images.githubusercontent.com/69246778/177756402-beafde75-f5ce-4c3d-9515-f8a3ae0ea3de.png)
런타임 다시 시작하라고 나옴. 

## 2.3. Dataset준비
ImageNet dataset을 사용한다. [dataset](http://image-net.org/)   
근데 백만장으로 훈련하는 건 에바라서 걍 평가만 할거임.   
```py
URL = "https://raw.githubusercontent.com/SharanSMenon/swin-transformer-hub/main/imagenet_labels.json" # Imagenet labels
!wget https://www.allaboutbirds.org/guide/assets/photo/306327661-480px.jpg -O house_finch.jpg
```
imageNet label jason파일이랑 새 사진 한 장 가져옴. house finch로 분류되는 사진임.

## 2.4. Evaluation
필요한 모듈 부르고
```py
import torch
import torchvision
import timm
from PIL import Image
```
   
timm의 model list에서 swin~이라는 이름을 가진 모델들의 리스트를 확인함.
```py
timm.list_models("swin*", pretrained=True) # This will list all the swin transformer models available
```   
![image](https://user-images.githubusercontent.com/69246778/177760305-62245044-4898-434b-8466-2f74abee0139.png)
나는 이 중에 사전 훈련된 swin-T(swin_tiny_patch4_window7_224)를 사용할 거임.
```py
model = timm.create_model('swin_base_patch4_window7_224', pretrained=True)
```

```py
print(model)
```
```py
SwinTransformer(
  (patch_embed): PatchEmbed(
    (proj): Conv2d(3, 96, kernel_size=(4, 4), stride=(4, 4))
    (norm): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
  )
  (pos_drop): Dropout(p=0.0, inplace=False)
  (layers): Sequential(
    (0): BasicLayer(
      dim=96, input_resolution=(56, 56), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=96, out_features=288, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=96, out_features=96, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): Identity()
          (norm2): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=96, out_features=384, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=384, out_features=96, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=96, out_features=288, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=96, out_features=96, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=96, out_features=384, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=384, out_features=96, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(56, 56), dim=96
        (reduction): Linear(in_features=384, out_features=192, bias=False)
        (norm): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
      )
    )
    (1): BasicLayer(
      dim=192, input_resolution=(28, 28), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=192, out_features=576, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=192, out_features=192, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=192, out_features=768, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=768, out_features=192, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=192, out_features=576, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=192, out_features=192, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=192, out_features=768, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=768, out_features=192, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(28, 28), dim=192
        (reduction): Linear(in_features=768, out_features=384, bias=False)
        (norm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
      )
    )
    (2): BasicLayer(
      dim=384, input_resolution=(14, 14), depth=6
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (2): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (3): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (4): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (5): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(14, 14), dim=384
        (reduction): Linear(in_features=1536, out_features=768, bias=False)
        (norm): LayerNorm((1536,), eps=1e-05, elementwise_affine=True)
      )
    )
    (3): BasicLayer(
      dim=768, input_resolution=(7, 7), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=768, out_features=2304, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=768, out_features=768, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=768, out_features=3072, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=3072, out_features=768, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=768, out_features=2304, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=768, out_features=768, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=768, out_features=3072, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=3072, out_features=768, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
    )
  )
  (norm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
  (avgpool): AdaptiveAvgPool1d(output_size=1)
  (head): Linear(in_features=768, out_features=1000, bias=True)
)
```
모델 아키텍쳐를 확인해보면 Swintransformr 블럭으로 구성된 총 4개의 stage(basic layer)가 있음.
