---
layout: post
title: About AI
description: |
  Vision Recognition
hide_image: true
tags:
  - deeplearning
published: true
---

# 객체인식 개요
* * *

# 1. 객체인식이란?
CNN를 사용해 사진의 Classification, Semantic Segmentation할 수 있음. 전자는 사진 단위로 인식해 이 사진이 뭔지를 분류하는 거고 
후자는 픽셀 단위로 인식하는 것을 말함. 객체인식이란, 사진이나 동영상에서 어떤 특정 개체들을 **인식**하고 분류를 통해 특징, 위치를 확인하는
복합적인 문제.
![image](https://user-images.githubusercontent.com/69246778/129155840-854edd54-baec-4ede-8583-43dc2be6f7cf.png)

# 2. Segmentation
* Instance Segmentation : 객체 단위로 실제 분할(어려움)   
* Semantic Segmentation : 객체가 아닌 전체적 덩어리로 이해   
   
![image](https://user-images.githubusercontent.com/69246778/129156327-6b204e3c-6e45-473e-a0f1-f65872a28602.png)   
   

# 3. Image Classification
사진 전체에 있는 어떤 특정 사물을 찾거나 분류하는 것. 즉, 사진 단위의 분류를 함. 이 떄 필요한게 CNN임. 
사진이 input으로 들어왔을 때 CNN에 의해 선형적, 비선형적 조합을 통해 feature를 추출함. 공간적인 특징에 의해 추출된 feature들이
vector로 표현되는데 이게 Class와 연결되면서 이 사진이 어떤 분류에 해당하는지를 정량적으로 예측하면서 Class score가 가장 큰
것으로 분류함.
![image](https://user-images.githubusercontent.com/69246778/129157060-7a637b30-2184-4429-823a-1c601cb8bb9b.png)

# 4. Object Detection으로 해결할 수 있는 문제 - 단일 객체 인식
Classification과 Localization을 동시에 고려하는 것. 만약 사진 한 장에서 고양이 하나를 객체인식하기 위해선 먼저 고양이가 있다는 걸 인식하는
**분류**의 문제가 해결되어야 하고, 더불어서 고양이의 주변을 고려하여 실제로 어디에 있는지 **위치**를 예측하는 것이 중요함. 
이걸 CNN이 해결함. 즉, 우리가 해결할 문제는 **분류와 위치** 두 개임. 이 두 가지를 동시에 고려하는 Multitask Loss를 설정하고 
이를 만족시키는 가중치를 학습을 통해 찾음.

# 5. Object Detection으로 해결할 수 있는 문제 - 다수의 객체 인식
인식해야할 객체가 다수이므로 너무 많은 Box가 생기고 이걸 전부 CNN에 통과시키면서 결과값을 확인하는 건 비효율적임. 박스가 어디서, 어떤 크기로
발생할지 알 수 없어서 이걸 다 고려하는 건 비효율적임. 효율적인 방안이 필요함.

