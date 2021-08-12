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
Selective Search로 2000여개의 후보군들이 추리고 같은 크기의 CNN을 적용할거기 때문에 추려진 영역들의 크기를 통일시킴. 
통일된 것들을 각각의 CNN에 대응하여 나오는 출력값으로 객체의 실제 위치를 예측함. 문제는 CNN을 2000번 돌려야한다는 것. 사진 한 장당 
2000번 돌리는건 연산량이 너무 많아서 느림. 이를 해결하기 위해 등장하는 것이 Fast R-CNN

# 3. Fast R-CNN
R-CNN에서 과정의 순서를 바꾸는 구조적인 변경을 통해 기존 R-CNN보다 빠른 신경망을 구축함. CNN을 먼저하고 그 다음에 뽑혀진 특징들에 대해
Region proposals을 함. 그니까 CNN은 한 번만 해도 되는거

## 3.1. RoI 추출
Region of Interest, CNN으로 뽑혀진 특징을 대상으로 추출함. 객체가 있을법한 실제 위치. 특징 공간 안에서도 실제 객체위치가 보존되므로
효율적으로 객체인식을 수행함.
격자 단위의 특징 공간에서 Max-pooling 사용해 최대값을 통해 산출되는 특징공간의 ROI를 추리
![image](https://user-images.githubusercontent.com/69246778/129175950-c06069db-9baf-41c9-b74c-506c497b9bfc.png)

## 3.2. RoI 정렬
실제 입력공간과 특징공간의 영역 위치가 어느정도는 유지되지만 정확히 일치하지 않을 수 있음. 이런 경우 선형보간법을 활용해 
실제 위치 값들의 중간값을 추정함. 이 추정한 값들이 실제보다 조금 틀어져있는 경우 Max-pooling으로 잘 맞춰줌
![image](https://user-images.githubusercontent.com/69246778/129176319-20de89fe-2094-43c3-9656-5e2ce42f50a3.png)

# 4. R-CNN과 Fast R-CNN비교
R-CNN은 객체 후보를 선정, 각 후보에 대해 CNN을 각각 실행해야하므로 느림. Fast R-CNN은 CNN을 한 번 수행하고 그 다음 RoI를 찾아 분류하므로
CNN을 한번만 해도돼서 더 효율적임. Fast R-CNN보다 빠른 Faster R-CNN까지 등장하게됨. 

# 5. Faster R-CNN
특징들에서 영역을 제안하는 영역 제안망(RPN)삽입. 선택적 탐색에서 영역 제안망으로 대체된 것. 그 외는 Fast R-CNN하고 비슷함.

## 5.1. RPN(Region Proposal Network)
CNN을 통해 나오는 출력값이 특징맵(격자형태)으로 처리되는데 이 격자형태의 각 점마다 앵커박스내에 객체가 있는지를 예측함.
![image](https://user-images.githubusercontent.com/69246778/129177985-2e025937-6452-47a3-a65c-91a1d354f03b.png)   
   
객체가 있을 확률이 높은(로지스틱 회귀) 앵커박스가 있는 위치가 실제 위치일 확률이 높음. 이게 주요한 후보가 될 것. 
즉, RPN은 RoI를 추출하고 객체를 찾는 과정. 앵커박스는 여러가지 크기를 가질 수 있음.

## 5.2. Faster R-CNN의 손실 함수
4가지의 손실함수 고려함.
* RPN의 객체 유무 손실
* RPN의 위치 박스 회귀 손실
* 각 객체들의 최종 분류 손실
* 최종 객체 위치 박스 회귀 손실

## 5.3. Faster R-CNN 동작
- 1단계 : 사진 단위로 수행, CNN, RPN
- 2단계 : 영역 단위로 수행, RoI 요약 및 정렬, 객체 분류와 위치 예측
최근에는 이 두 단계를 한 단계로 줄인게 SSD, YOLO, RetinaNet 같은 것들이 등장. 

## 5.4. 단일 단계 객체인식으로 변화
입력 자체를 격자 형태로 임의로 나누고 이 입력의 격자마다 앵커박스를 추정하고 각 분류 예측 결과를 제시함. input에 바로 적용하니까 간단해짐

# 6. 정리
근간이 되는 인공 신경망에는 VGG16, ResNet/ResNet101, Inception/Inception V2/Inception V3, Mobile Net등이 있다.
인공신경망의 구조적 단계에 따라 나눌 수도 있는데, 2단계인 Faster R-CNN이 있고 단일 단계인 YOLO/SSD/RetinaNet 그리고 혼합단계인 R-FCN.
대표적인 인공신경망을 정리하면 다음과 같음

|인공신경망 | 특징|
|:---------|:----|
|Faster R-CNN|느리지만 높은 정확도, 사진에 적합|
|SSD|빠르지만 낮은 정확도, 동영상에 적합|


