---
layout: post
title: 
description: |
  CSWin Transformer: A General Vision Transformer Backbone with Cross-Shaped Windows, 9 Jan 2022, University of Science and Technology China, Microsoft.
hide_image: true
tags:
  - review
published: true
---

# CSWin Transformer: A General Vision Transformer Backbone with Cross-Shaped Windows
[Link](https://arxiv.org/abs/2107.00652)
* * *

# Abstract
CSwin Transformer는 전역 어텐션으로 발생하는 계산량 문제를 해결하기 위해 소개된 트랜스포머 기반 백본이다. Cswin은
**어텐션 영역을 십자가 모양으로 제한하고 LePE를 사용하여 성능을 향상시켰다.** 

# 1. Introduction
![image](https://user-images.githubusercontent.com/69246778/223364833-3c5fc46f-9402-469d-938b-7d1e7eca91bb.png)

트랜스포머 모델에서 효율성을 높이기 위한 한가지 방법은 각 토큰에 대해 어텐션 영역을 제한하는 것이다. 어텐션 영역끼리 연결하기 위해
halo나 shift등의 기술을 사용하면 윈도우 간 정보 교환이 가능하다. 충분히 큰 수용장이 성능향상에 도움이 도움이 되는데 수용장의 크기가
느리게 증가하면서 아주 많은 어텐션이 수행된다. 따라서, 중요한 것은 **충분히 큰 수용장을, 최소한의 비용으로 달성하는 것이다.**   
본 논문에서는 **Cross-shaped Window(CSwin) self-attention**을 제안한다. 기존 셀프 어텐션 매커니즘이랑 비교해보면, **수직, 수평
스트라이프에서 어텐션 연산을 병렬로 수행하는 것이 특징이다.** 이때, 스트라이프의 간격은 중요한 매개변수가 된다. 스트라이프 너비를
계산 비용을 조절하면서 높은 성능에 달성할 수 있기 때문이다. **얕은 레이어에는 작은 너비를 사용하고 깊어질수록 큰 너비를 
사용한다**   
병렬 연산을 수행하

# 2. Related Work
## 2.1. Vision Transformer
## 2.2. Efficient Self-attentions
## 2.3. Positional Encoding

# 3. Method
## 3.1. Overall Architecture
![image](https://user-images.githubusercontent.com/69246778/192493775-faa94551-5c89-4d79-b8be-ddad15716310.png)   
<**Stage1**>   
**Input Image : H * W * 3**   
**Conv layer : 7 * 7, Stride 4**   
**Output Patch token : H/4 *  W/4 * C**   
<**Conv layer**>   
**3 * 3, Stride 2**   
**Output feature map**

<**Stage2**>   


## 3.2. Cross-Shaped Window Self-Attention
### 3.2.a. Horizontal and Vertical Stripes
### 3.2.b. Computation Complexity
### 3.2.c. Locally-Enhanced Positional Encoding
## 3.3. CSwin Transformer Block
## 3.4. Architecture Variants
