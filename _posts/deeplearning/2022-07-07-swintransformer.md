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
