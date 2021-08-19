---
layout: post
title: YOLOv2
description: |
  YOLOv2를 활용한 Multi objet Detector
hide_image: true
tags:
  - ai
published: true
---


# 1. Training Data labeling
* * *
Machine Learning의 방법론 중 하나인 Deep Learning에서, 지도학습(Supervised Learning)을 하기 위해선, 주어지는 데이터에 대해 Label이 
필요하다. 지도학습이란, 기계에게 사진을 보여주고 이 사진에 대한 정답을 함께 알려주는 식으로 학습하는 것으로,
Label은 여기서 '정답'에 해당하는 것이다. 그러니까, 정답을 잘못알려주면 즉 Label을 정확히 하지 못한다면 모델의 성능이 떨어질 수 
있으므로 정확한 Label이 중요하다. 기계를 학습시키는 데에 사용하는 Training data에 labeling한 것을 학습에 활용하고, 
학습을 마치면 Test data를 이용해 학습이 잘 되었는지 평가하게 된다. 따라서, 학습의 첫 단계는 Training data에 정확한 labeling을 
하는 것이라 할 수 있겠다.   
   
Training Data를 학습시키기 위해 KITTI의 left color images of object data set를 사용한다. image dataset을 다운로드받고
training labels of object data set을 다운로드 한다.

![image](https://user-images.githubusercontent.com/69246778/129994047-e0e7fda3-5777-4803-b022-f9808f6ae6ef.png)
   
![image](https://user-images.githubusercontent.com/69246778/129994099-0f54164e-7886-42ba-9696-d7462e5bb8df.png)
이렇게 image data set을 저장했는데, 사정상 노트북으로 학습해야하고 project 기한이 짧아 이 중 일부 사진만 학습할 예정이다.
