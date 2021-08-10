---
layout: post
title: About AI
description: |
  ANN(Artificial Neural Network)와 Deep learning
hide_image: true
tags:
  - ai
published: true
---

# 1. ANN과 Deep Learning
* * *

## 1.1. ANN(Artificial Neural Network)
인간의 신경망(뇌)에서 영감을 얻은 통계학적 학습 알고리즘. 시냅스의 결합으로 네트워크를 형성한 인공 뉴런(node)이 학습을 통해
시냅스의 결합 세기를 변화시켜 문제 해결 능력을 가지는 모델 전반을 말함.   
![image](https://user-images.githubusercontent.com/69246778/128827637-e74578b6-b35f-458a-81cb-6e5ebe487520.png)   
Deep Learning은 인공신경망이 deep해지면서 레이어가 많아진 것. 인공지능을 이루기 위한 Maching Learning이라는 방법론 중 하나가 딥러닝.
   
## 1.2. Deep Learning의 비약적 발전

### 1.2.1. Algorithm의 발전
over-fitting과 Vanishing Gradient(기울기 소실)을 해결. 

#### 1.2.1.A. Algorithm의 발전 - 비지도학습을 이용한 전처리
가중치 감쇠 규제
#### 1.2.1.B. Algorithm의 발전 - Convolutional Neural Network(합성곱 신경망)
#### 1.2.1.C. Algorithm의 발전 - Rectified Linear Unit
새로운 활성화함수 ReLU의 도입
#### 1.2.1.D. Algorithm의 발전 - Drop-out
어떤 신경망이 over-fitting을 피하기 위해 학습과정의 결과를 일부러 버리는 아니러니컬한 idea.

### 1.2.2. Hardware의 발전
#### 1.2.2.A. 범용 GPU
병렬적인 연산 수행에 사용. 학습시간 대폭 단축

### 1.2.3. 빅데이터의 등장
ANN은 Weight의 수가 많아 이를 모두 학습시키기에는 많은 양의 data가 필요. Big data의 등장으로 인공신경망이 deep할 때도 충분히 학습시킬 수
있어서 성능 향상됨.

## 1.2. ANN의 구조
![image](https://user-images.githubusercontent.com/69246778/128829602-13a7ba4a-02cc-4c98-99b4-b0c2f6d41f2c.png)

### 1.2.1. 퍼셉트론
ANN을 이루는 기본 구조.
![image](https://user-images.githubusercontent.com/69246778/128829953-054dd8e4-ac24-4fb0-b9e7-bc2b8bd5f5a0.png)   
x1부터 xd까지 d차원의 input을 받을 때, 이 각각의 d차원 input에 곱해지는 가중치가 w1부터 wd까지 존재함.
이 w와 x를 곱한 것들의 합이(즉, 벡터w와 벡터x의 내적) Threshold value보다 크면 1, 작으면 0으로 y값을 결정.
이렇게 결정해주는 함수를 활성화함수 h라 함. h는 set function임.
![image](https://user-images.githubusercontent.com/69246778/128831273-22eea691-0cd3-4721-a956-e2146604c578.png)

### 1.2.1.A. 논리gate의 구현


