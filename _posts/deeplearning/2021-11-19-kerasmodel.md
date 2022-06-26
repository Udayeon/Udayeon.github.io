---
layout: post
title: 
description: |
  keras Sequential model 만들기
hide_image: true
tags:
  - deeplearning
published: false
---

(python) keras Sequential model만들기

# tf.keras.models.Sequential 
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

## Dense layer
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
```py
Dense(output크기=16, 활성화함수=relu, 이름=layer_1)
```
결과
![image](https://user-images.githubusercontent.com/69246778/142569222-502f3a1d-7891-4dac-9200-84f22d6b1071.png)
그림으로 나타내면 다음과 같음
![parameter](https://user-images.githubusercontent.com/69246778/142571355-e62457fe-1f2d-4f8b-8711-a4dcc14a84bb.jpg)

## Convo2D, MaxPooling2D
```py
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
import tensorflow as tf

#Building a Sequential model
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense,Flatten,Softmax, Conv2D, MaxPooling2D

#Build a Convolutional neural network model
model=Sequential([
    Conv2D(16, (3, 3), activation='relu', input_shape=(28, 28, 1)),
    MaxPooling2D((3, 3)),
    Flatten(),
    Dense(10, activation='softmax')
])

model.summary()
```

* Conv2D : 2D Convolutional layer
* MaxPooling2D : 2D maxpoolling layer
```py
Conv2D(output=16, kernel size=3x3, 활성화함수=relu, input=28x28x1(width,height,depth))
MaxPooling2D(kernel size=3x3)
```
   
결과
![image](https://user-images.githubusercontent.com/69246778/142573000-3e2390cd-645f-438e-af79-00a89130018f.png)
![convpooling](https://user-images.githubusercontent.com/69246778/142574150-1799c52e-53e9-4050-bdc7-a6880cf813f0.jpg)

# model.compile
```py
#Compile the model
opt =tf.keras.optimizers.Adam(learning_rate=0.005)
acc = tf.keras.metrics.SparseCategoricalAccuracy()
mae = tf.keras.metrics.MeanAbsoluteError()

model.compile(optimizer=opt,
              loss='sparse_categorical_crossentropy',
              metrics=[acc,mae])
```

* adam
Adaptive Moment Estimation.
[참고논문](https://arxiv.org/pdf/1609.04747.pdf)
[참고](https://hiddenbeginner.github.io/deeplearning/2019/09/22/optimization_algorithms_in_deep_learning.html)

* sparse_categorical_crossentropy
[참고](https://www.tensorflow.org/api_docs/python/tf/keras/metrics/sparse_categorical_crossentropy)
다중분류 손실함수. 정수 타입의 클래스를 one-hot encoding하지 않고 정수 형태 그대로 라벨링

* metrics
학습과 테스트 과정에서 모델이 평가할 측정항목의 리스트. 
보통은 metrics=['accuracy']를 사용하면 됨.
다중 아웃풋 모델의 각 아웃풋에 각기 다른 측정항목을 특정하려면, metrics={'output_a': 'accuracy'}사용.
