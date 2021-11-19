---
layout: post
title: (python) model.compile
description: >
  
tags: [programming]
author: Udayeon
---

# (python) model.compile
model을 compile해줌
```py
model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])
```
[optimizer,loss,metrics참고](https://keras.io/ko/models/model/)

## adam
Adaptive Moment Estimation.
[참고논문](https://arxiv.org/pdf/1609.04747.pdf)
[참고](https://hiddenbeginner.github.io/deeplearning/2019/09/22/optimization_algorithms_in_deep_learning.html)

## sparse_categorical_crossentropy
[참고](https://www.tensorflow.org/api_docs/python/tf/keras/metrics/sparse_categorical_crossentropy)
다중분류 손실함수. 정수 타입의 클래스를 one-hot encoding하지 않고 정수 형태 그대로 라벨링

## metrics
학습과 테스트 과정에서 모델이 평가할 측정항목의 리스트. 
보통은 metrics=['accuracy']를 사용하면 됨.
다중 아웃풋 모델의 각 아웃풋에 각기 다른 측정항목을 특정하려면, metrics={'output_a': 'accuracy'}사용.
