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
!python version
```



