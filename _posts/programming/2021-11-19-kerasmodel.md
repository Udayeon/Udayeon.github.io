---
layout: post
title: (python) keras Sequential model만들기
description: >
  
tags: [programming]
author: Udayeon
---

# (python) keras Sequential model만들기

## tf.keras.models.Sequential 
```py

import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
import tensorflow as tf

model = tf.keras.models.Sequential([
  tf.keras.layers.Dense(16, activation='relu'),
  tf.keras.layers.Dense(10, activation='softmax')
])
```
**unit=16, 활성화함수로 relu 함수를 사용하는 첫 번째 레이어 + unit=10,활성화함수로 softmax를 사용하는 두 번째 레이어**를 순차적으로
더한 모델을 만든다.

## model.summary()로 살펴보기
```py
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
import tensorflow as tf

#Building a Sequential model
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense,Flatten,Softmax

#Build a feedforward nerual network model
model = Sequential([
    Flatten(input_shape=(28,28)),
    Dense(16,activation='relu',name='layer_1'),
    Dense(16,activation='relu'),
    Dense(10),
    Softmax()
])
```
* Dense : 완전 연결층
   
결과
![image](https://user-images.githubusercontent.com/69246778/142569222-502f3a1d-7891-4dac-9200-84f22d6b1071.png)
그림으로 나타내면 다음과 같음
![parameter](https://user-images.githubusercontent.com/69246778/142571355-e62457fe-1f2d-4f8b-8711-a4dcc14a84bb.jpg)
