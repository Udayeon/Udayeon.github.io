---
layout: post
title: 
description: |
  Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun
  Microsoft Research
hide_image: true
tags:
  - review
published: true
---

# Deep Residual Learning for Image Recognition

[Link](https://arxiv.org/pdf/1512.03385.pdf)
* * *

## Abstract
신경망이 깊어질수록 학습이 어려워진다. 본 논문에서는 layer의 인풋을 참조하는 residual function을 학습하는 방식으로 깊은 
네트워크에서의 정확도를 향상시킨다. 

## 1. Introduction
합성곱 신경망이 깊어질수록 높은 성능을 달성할 수 있다. 그러나 레이어를 무한정 적층할 수는 없다. 왜냐하면, 
**Gradient vanishing/exploding**문제가 발생하기 때문이다. 이 문제들은 normalized initialization이나
intermediate normalization layers를 통해서 어느정도는 해결할 수는 있지만, layer가 아주 많아지면 한계가 존재한다.
아래 그림을 보면, CIFAR데이터에 대해 오히려 레이어가 깊어질수록 에러가 큰 것을 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/69246778/223010148-47a2ccdd-1ec9-4d86-aa87-a2b4ead3ce17.png)   
   
이는 ImageNet에서도 마찬가지다.   
![image](https://user-images.githubusercontent.com/69246778/223010400-e7ef2bd5-c406-4ee8-aa74-78591ed6a131.png)   
   
이는 오버피팅의 문제는 아니다. 최적화의 문제라고 볼 수 있는데, 네트워크가 깊어질수록 최적화하기(train) 어렵기 때문이다. 본 논문에서는
**deep residual connection**을 제안하여 정확도 감소 문제를 해결한다. 아래의 그림은 residual connection을 나타내는 그림이다.      
![image](https://user-images.githubusercontent.com/69246778/223305539-b84aba79-1fb7-427e-8f5b-a523c1118757.png)   
F(x)+x는 **shortcut connections**을 가진 피드포워드 신경망으로 실현할 수 있다. shortcut connection은 하나 이상의 레이어를 건너
뛰는 것을 말하는데 위의 그림에서는 identity mapping하는 부분이 shortcut이고 그 출력을 layer를 거쳐 나온 출력에 더한다. 즉, 기존의 학습 정보를
보존하고 거기에 추가적으로 학습할 정보를 더해주는 식으로 추가적으로 학습해야 할 정보만을 학습하는 것.


## 2. Related Work
* **Residual Representations**   
VLAD와 fisher vector는 이미지 탐색과 분류에 대해 파워풀한 shallow representation이다. 또한, vector quantization은 residual
vector를 인코딩하는 것이 원래 벡터를 인코딩하는 것보다 효과적임을 입증한다.   
low-level의 컴퓨터 비전에서 사용되는 Multigrid방식은 시스템을 여러 개의 작은 문제들로 재정의 하는데, 각 문제들은 coarser한 
스케일과 finer한 스케일 간의 residual(잔차)를 처리한다. 이런 방식은 residual을 계산하지 않는 솔루션보다 수렴 속도가 훨씬 빠르다.   
   
* **Shortcut Connections**   
shortcut connection은 신경망에서 한개 이상의 레이어를 건너뛰고 연결하는 방식이다. MLP를 학습하는 초기 방법 중 하나는 네트워크 
입력에서 출력으로 선형 레이어를 추가하는 것이다. 또한, 중간에 있는 레이어가 보조 분류기에 직접 연결되어 그래디언트 소실/폭발 문제를
해결하기도 한다.   
'highway network"는 gating function을 가진 shortcut을 제안한다. 이 게이트는 데이터에 따라 달라지며 매개 변수를 갖고 있는데,
매개변수가 없는 것은 Identity shortcut(본 논문에 쓰인 방식)이라 한다.   
   
## 3. Deep Residual Learning
### 3.1. Residual Learning
네트워크 초기 입력을 x라 할때 몇몇 레이어를 통해 매핑한 값을 H(x)라 할 수 있다. (네트워크 전체를 거친 출력을 의미하는 것은 아님) 
그리고 다른 몇몇 레이어에서는 f(x) = H(x)-x라는 잔여 함수(입력과 출력의 차이)를 mapping한다. 
따라서, 원래 함수 H(x) = f(x) + x가 된다. residual 매핑을 최적화 하는 것이 원래 mapping을 최적화하는 것보다 더 쉽다는 가설을 
세웠고, 만약 identity 매핑이 최적이라면, residual을 0으로 밀어버리면 된다. 실제로 identity가 최적일 가능성은 적지만, 만약
최적 함수가 제로 매핑보다 identity 매핑에 더 가깝다면 새로운 함수를 학습하기 보다 identity기준으로 노이즈를 찾는 것이 쉽기
때문에 Identity매핑이 합리적인 사전 조건을 제시한다고 할 수 있다. 
### 3.2. Identity Mapping by Shortcuts
본 논문에서는 모든 layer층마다 residual learning을 적용한다. fig2의 블럭을 쌓는 방식으로 구현한다.   
![image](https://user-images.githubusercontent.com/69246778/223346615-bd1e276b-0372-4d07-8345-79437c0f1ec3.png)    
이 식이 fig2의 식인데, x와 y는 각각 인풋,아웃풋 벡터로 residual mapping F(x,w)와 identity mapping x를 더한다.   
![image](https://user-images.githubusercontent.com/69246778/223346674-28ec217e-33c8-4530-a6c7-adb82ee100b1.png)   
identity에 가중치를 줄수도 있다. 그치만, 그냥 기본적인 identity만으로도 정확도 감소 문제 해결에 충분하다.   
함수 F는 그 형태가 매우 다양하고 유연하며 fully-connected layer뿐만 아니라 여러 합성곱 계층을 나타낼 수도 있다.

## 4. Experiments
![image](https://user-images.githubusercontent.com/69246778/223348601-ce7a96df-eafc-4dc5-bc9c-d54a36a3d8ef.png)
깊은 레이어를 가졌음에도 성능 향상됨.

## 논문요약
* “Deep Residual Learning for Image Recognition”은 딥 러닝에서 생기는 “degradation” 문제를 해결하기 위해 제안된 논문.    
* 층이 많은 딥 러닝 네트워크가 레이어를 추가할수록 성능이 저하되는 현상을 발견하였고, 이를 해결하기 위해 residual learning 개념을 도입.   
* Residual learning은 입력과 출력의 차이인 “residual”에 대한 매핑을 학습하는 것으로, 이를 통해 더 깊은 네트워크를 학습하면서도 성능 저하 문제를 해결. 
* 이때, residual mapping을 적용하기 위해 “shortcut connection”을 사용하며, 이는 identity mapping을 수행하는데 사용됩니다.   
* 실험 결과, residual learning을 적용한 네트워크가 이전 네트워크 대비 더 높은 성능을 보임.    
* 또한, 이 방법은 일반적인 딥 러닝 라이브러리를 이용하여 쉽게 구현할 수 있으며, 추가적인 파라미터나 계산 복잡도를 요구하지 않는다는 장점이 있다.   
