---
layout: post
title: Deep Neural Networks for Object Detection
description: >
  
tags:
  - review
use_math : true
comments : true
author: Udayeon

published: true
---

# Deep Neural Networks for Object Detection
[Christian Szegedy Alexander Toshev Dumitru Erhan (2013)](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41457.pdf)
* * *
DetectorNet은 object detection에 CNN을 처음으로 도입한 network

# 1. Introduction
* * *
**본 논문에서는 DNN을 이용해 객체를 분류하고 정확한 위치를 파악한다.**
AlexNet을 사용하는데, 마지막 layer의 softmax 활성화함수를 regression layer로 변경해 사용.   
주어진 이미지 내에서 다수의 object에 대한 바운딩박스를 예측.   
   
# 2. Related Work
* * *
본 논문과 가장 가까운 접근법을 가진 논문 [Object-Class Segmentation using Deep Convolutional Neural Networks](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.681.4658&rep=rep1&type=pdf#page=61)
비슷한 수준의 목표를 가졌지만 다른 feature와 손실함수를 사용하고 여러 instance는 구별하지 못함.

# 3. DNN-based Detection
* * *
![image](https://user-images.githubusercontent.com/69246778/147900759-f30aebf4-054b-4bd1-9ca0-1ec9d055975c.png)
본 논문의 핵심 접근방식은 **fig 1**처럼 object mask를 향한 DNN 기반 회귀이다. 이 회귀 모델을 기반으로 객체의 일부 및 전체에 대해
mask를 생성할 수 있다. 단일 DNN 회귀는 하나의 이미지 내의 여러 object에 대한 mask를 제공한다. 더 정밀한 위치파악을 위해,
큰 subwindows 하위에 DNN localizer를 적용한다. 

# 4. Detection as DNN Regression
* * *

