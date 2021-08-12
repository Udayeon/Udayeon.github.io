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

# 객체인식을 위한 깊은 인공신경망
* * *
하나의 사진, 입력에 여러 객체가 존재할 때 비효율적인 문제가 발생함. 객체가 어떤 위치에 어떤 크기로 발생할지 예측할 수 없기 때문에 모든 영역을
다 찾는 것은 불가능함. 이를 해결하는 방안들이 있음

# 1. Selective Search
실제적으로 객체가 발생할 수 있거나 객체가 있을 법한 위치를 추정해서 후보군을 추리는 방법. R-CNN, Fast R-CNN, SPPNets에 사용됨.
상대적으로 빠르게 수행되는 알고리즘임. 실제로 CPU로 한 장의 사진에서 2000개의 부분을 제안하는 데에 수초 밖에 안걸림.

# 2. R-CNN
Selective Search로 2000여개의 후보군들 추려지는데, 이 후보군들을 추릴 때 아까 전의 Segmentation처럼 
