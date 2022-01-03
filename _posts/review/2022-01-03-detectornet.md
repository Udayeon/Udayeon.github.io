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
DetectorNet은 object detection에 CNN을 처음으로 도입한 network중 하나임.
one stage로 동작.

# 1. Introduction
* * *
AlexNet을 사용하는데 마지막 layer의 softmax 활성화함수를 regression layer로 변경해 사용.   
주어진 이미지 내에서 다수의 object에 대한 바운딩박스를 예측.   
이미지 foreground pixles예측을 위한 네트워트 1개 + 위,아래,오른쪽,왼쪽 예측을 위한 추가 네트워크 4개 이용 = 5개   
이미지를 여러번 crop하는데 이 crop된 것마다 여러 네트워크를 수행해야해서 속도가 느림

# 2. Related Work
* * *
본 논문과 가장 가까운 접근법을 가진 논문 [Object-Class Segmentation using Deep Convolutional Neural Networks](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.681.4658&rep=rep1&type=pdf#page=61)
비슷한 수준의 목표를 가졌지만 다른 feature와 손실함수를 사용하고 여러 instance는 구별하지 못함.

# 3. DNN-based Detection
* * *
![image](https://user-images.githubusercontent.com/69246778/147900759-f30aebf4-054b-4bd1-9ca0-1ec9d055975c.png)
본 논문의 핵심 접근방식은 **fig 1**처럼 object mask를 향한 DNN 기반 회귀이다. 이 회귀 모델을 기반으로 객체의 일부 및 전체에 대해
mask를 생성할 수 있다. 단일 DNN 회귀는 하나의 이미지 내의 여러 object에 대한 mask를 제공한다. 
더 정밀한 위치파악을 위해, 큰 subwindows 하위에 DNN localizer를 적용한다. (여러 네트워크가 필요한 셈)

# 4. Detection as DNN Regression
* * *
여러 scale과 커다란 이미지 box에 걸쳐 object mask를 **regression**분석 한 후에, **object box 추출**을 수행함. 추출된 상자는 하위 이미지에 대해서 같은 과정을
반복하면서 수정되고, 현재 object box에 의해 crop된다. **fig 2**는 간략한 표현을 위해 객체의 forground pixel에 대한 box만을 나타냈지만 실제로는 
5가지(전체,위,아,왼,오) mask를 모두 사용함. 

# 8. Conclusion
* * *
너무 많은 네트워크를 사용하므로 시간이 오래걸림. 단일 네트워크를 사용할 필요가 있음.
