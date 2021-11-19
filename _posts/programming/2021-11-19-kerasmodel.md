---
layout: post
title: (python) tf.keras.models.Sequential
description: >
  
tags: [programming]
author: Udayeon
---

# (python) tf.keras.models.Sequential
keras로 순차모델을 만든다. 순차모델은 layer를 순차적으로 더하는 모델을 말함.   
```py
model = tf.keras.models.Sequential([
  tf.keras.layers.Dense(16, activation='relu'),
  tf.keras.layers.Dense(10, activation='softmax')
])
```
   
**unit=16, 활성화함수로 relu 함수를 사용하는 첫 번째 레이어 + unit=10,활성화함수로 softmax를 사용하는 두 번째 레이어**를 순차적으로
더한 모델을 만든다. 이때 unit은 출력 값의 크기를 의미한다.   


   
