---
layout: post
title: Deep Learning without Poor Local Minima
description: >
  
tags:
  - review
use_math : true
comments : true
author: Udayeon

published: true
---

# Deep Learning without Poor Local Minima
[Kenji Kawaguch. arXiv:1605.07110 (2016).](https://arxiv.org/abs/1605.07110v3)
* * *

# Abstract
본 논문에서는 먼저, **deep linear neural network**의 **squared loss function**에 대한 다음과 같은 진술을 입증한다.
```
1) squared loss function은 non-convex하고 non-concave하다.
2) 모든 local minima는 global minima이다.
3) global minima가 아닌 모든 극값은 saddle point이다.
4) 깊은 네트워크에는 '나쁜' saddle point가 있지만, 얕은 네트워크에는 그런게 없다.
```
**deep nonlinear neural network**에 대해서도 위와 같은 4개의 진술을 입증할 수 있는데, 이때는 **independence assumption**
하에서 deep linear model로의 축소를 사용한다.   
결과적으로, 우리는 다음과 같은 질문들에 답할 수 있게된다.
```
이론적으로, deep model을 직접 훈련시키는 것이 얼마나 어려울까?
```

# Introduction
