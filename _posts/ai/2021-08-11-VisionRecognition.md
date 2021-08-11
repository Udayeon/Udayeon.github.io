---
layout: post
title: About AI
description: |
  Vision Recognition
hide_image: true
tags:
  - ai
published: true
---

# 1. 객체인식 개요
* * *

# 2. 객체인식을 위한 깊은 인공 신경망
* * *
객체 인식을 할 때 하나의 사진, 입력에 여러 객체가 존재하면 비효율적인 인식 문제가 발생함. 객체가 어디서 발생할지, 어떤 크기일지 예측하지 못하기 
때문에 모든 영역을 다 찾는 것은 비효율적임. 이걸 해결하는 인공신경망이 있음.

## 2.1. Selective search
input에서 객체가 발생할 수 있거나 객체가 있을 법한 위치를 실제적으로 추정해서 그것들의 후보군을 제안하는 방법 중 하나.
R-CNN, Fast R-CNN, SPPNets과 같은 객체 인식 모델에 실제적으로 활용되는 기술임. CPU가 한 장의 사진에서 객체가 있을 법한 부분을 2000여개 정도 
제안하는 데에 몇 초 밖에 안걸릴 정도로 알고리즘 속도가 빠름.

## 2.2. R-CNN
Selective search로 후보군을 추
