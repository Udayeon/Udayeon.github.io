---
layout: post
title: 
description: |
  Backbone, Neck, Head에 대하여
hide_image: true
tags:
  - deeplearning
published: true
---

# Backbone, Neck, Head
* * *

# 1. Backbone
입력 이미지를 feature map으로 변환시켜주는 부분   
**ResNet, Darknet, VGG, Swin transformer, ViT**

# 2. Neck
Backbone과 head를 연결짓는 부분. feature map을 정제(refinement) 또는 재구성(reconfiguration)한다.      
**FPN, PAN**

# 3. Head
Backbone에서 추출한 feature map의 locaton 작업을 수행하는 부분. predict classes와 bounding boxes 작업.   

## 3.1. Sparse Prediction head
**2-stage detector**(predict classes와 bounding boxes 작업이 분리되어 있다.)에서 사용     
**Faster R-CNN**
## 3.2. Dense Prediction head
**1-stage detector**(predict classes와 bounding boxes 작업이 통합되어 있다.)에서 사용   
**YOLO, SSD**


# 4. 예시
## 4.1. YOLO v4
![image](https://user-images.githubusercontent.com/69246778/176678827-2753a244-f750-4154-b7ad-92374012730f.png)
1) Backbone : CSP-Darknet53
2) Neck : SPP(Spatial Pyramid Pooling), PAN(Path Aggregation Network)
3) Head : YOLO-v3

# 5. 요약
![image](https://user-images.githubusercontent.com/69246778/176684168-b4bd1555-87e9-4719-8b04-c1182ae25845.png)
