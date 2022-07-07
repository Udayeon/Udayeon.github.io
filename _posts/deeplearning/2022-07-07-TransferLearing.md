---
layout: post
title: 
description: |
  Transfer Learning - Pretrained된 network사용하기
hide_image: true
tags:
  - deeplearning
published: true
---

# Transfer Learning - Pretrained된 network사용하기
* * *

# 1. Transfer Learning 이란
정교한 딥 러닝 모델에는 수백만 개의 매개변수(가중치)가 있으며 이를 처음부터 훈련하려면 종종 많은 양의 컴퓨팅 리소스 데이터가 필요하다.    
전이 학습은 관련 작업에 대해 이미 훈련된 모델의 일부를 가져와 새 모델에서 재사용함으로써 손쉽게 데이터를 구축하는 기술이다.   
Computer Vision, Image classification분야에서는 torchvision이나 timm이 여러 SOTA 모델을 제공한다.   

# 1.2. Transfer Learning vs fine tuning
![image](https://user-images.githubusercontent.com/69246778/177728647-c10127d3-7e53-44f5-b0ab-56cd46bf61b0.png)
[출처](https://inhovation97.tistory.com/31)


# 2. torchvision
## 2.1. 필요한 모듈 설치
```py
!pip3 install torch
!pip3 install torchvision
```
## 2.2. 모델 불러오기
```py
import torchvision
import torch

model=torchvision.models.resnet18(pretrained=True)

data=torch.Tensor(1,3,224,224) # 1 batch_size, 3 channels(RGB), 224 width, 224 height
output=model(data)
print(output.shape)
```
```py
>> torch.Size([1, 1000])
```
이 때 불러온 모델은 **resnet18**로 **ImageNet**으로 pretrain됐기 때문에 **Class가 1000개**임.   

## 2.3. class 10개로 분류하는 모델로 수정하기
```py
import torch.nn as nn

num_classes=10
model.fc=nn.Linear(model.fc.in_features,num_classes,bias=True)

data=torch.Tensor(1,3,224,224)
output=model(data)
print(output.shape)
````
fc layer를 바꿔주면 된다.
```py
>> torch.Size([1,10])
```

## 3. timm

