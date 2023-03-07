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
뛰는 것을 말하는데 위의 그림에서는 identity mapping하는 부분이 shortcut이고 그 출력은 layer를 거쳐 나온 출력에 더해진다.   


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

