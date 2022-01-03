---
layout: post
title: Deep Neural Networks for Object Detection
description: >
  
tags:
  - review
use_math : true
comments : true
author: Udayeon

published: true
---

# Deep Neural Networks for Object Detection
[Christian Szegedy Alexander Toshev Dumitru Erhan (2013)](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41457.pdf)
* * *
DetectorNet은 object detection에 CNN을 처음으로 도입한 network중 하나로써 one stage로 동작하는 모델이다. 

two-stage : 분류와 위치파악을 따로 수행
one-stage : 분류와 위치파악을 한 번에 수행
{:message}

DetectorNet은 AlexNet을 사용하는데 마지막 layer의 softmax 활성화함수를 regression layer로 변경해 사용하는 것이 특징이다.
AlexNet의 구조는 다음과 같다. 
![image](https://user-images.githubusercontent.com/69246778/147906216-e5522d9e-693a-4689-9c34-126915da87b0.png)
   
주어진 이미지 내에서 다수의 object에 대한 바운딩박스를 예측하기 위해 5개의 네트워크를 사용한다.
**이미지 foreground pixles예측을 위한 네트워트 1개 + 위,아래,오른쪽,왼쪽 예측을 위한 추가 네트워크 4개 이용 = 5개**   
그림으로 보면 아래와 같음.
![image](https://user-images.githubusercontent.com/69246778/147900759-f30aebf4-054b-4bd1-9ca0-1ec9d055975c.png)   
   
전체적인 작동원리는 아래 그림과 같은데 간략하게 나타낸 그림이고 실제로는 5개의 네트워크(전체,위,아,오,왼)를 모두 수행해야함.
다양한 scale의 이미지들에 걸쳐 **regression**을 수행한 후 **object box를 추출**함. 그렇게 얻어진 box에 대해 또다시 과정을 반복하면서 **refine**하게 됨. 
![image](https://user-images.githubusercontent.com/69246778/147906437-62570375-f80c-4857-b647-0a15be2e2463.png)
(그리고 또 다른 특징은 이미지당 40개 미만의 적은 수의 window만을 평가한다는 것이다. 그런 점에서 sliding window 방식이라고 보기는 어렵다.)   
   
input이미지를 여러번 crop하게 되는데 이 crop된 것마다 여러 네트워크를 수행해야 하니까 속도가 느려진다.
결론적으로 CNN을 적용한 것엔 의의가 있지만 시간이 너무 오래 걸리고 mAP도 낮게 나오기에 비효율적임.
실험결과는 다음과 같음. 기존의 방식에 비해선 성능이 개선되었지만 이후에 등장한 R-CNN에 비해선 턱없이 낮음.
![image](https://user-images.githubusercontent.com/69246778/147906834-d92f72c7-b531-451c-86d4-1bd8f2b85702.png)
   
**단일 네트워크를 사용할 필요가 있어보임**

