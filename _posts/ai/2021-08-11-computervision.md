---
layout: post
title: About AI
description: |
  Computer Vision
hide_image: true
tags:
  - ai
published: true
---

# Computer Vision
* * *

# 1. Computer Vision 이란?
**Classification, Retrieval, Detection, Segmentation**에 활용. 단순히 동작을 인식하는 것 뿐 아니라, 이 동작이 뭔 의미인지 이해하고
사진을 보여주면 이 사진이 뭔지도 컴퓨터가 판단하는 경지에 다다름. 최종적으로 원하는 것은, 컴퓨터가 학습을 완료한 후에 새로운 영상이 주어졌을 때
뭔지 맞추는 것.
![image](https://user-images.githubusercontent.com/69246778/128991485-acbaac17-e1ab-41e7-bc75-edd7a84e0161.png)

# 2. Computer Vision이 어려운 이유
* 동일한 객체라도 영상을 찍는 카메라의 이동에 따라 모든 픽셀값이 변화되므로 데이터가 달라짐.   
* 경계색(보호색)으로 배경과 구분이 어려운 경우   
![image](https://user-images.githubusercontent.com/69246778/128991929-085b88e4-6869-4ca8-9d2d-1e7f800fa896.png)   
   
* 조명의 변화로 저장값들이 변함
![image](https://user-images.githubusercontent.com/69246778/128992007-3f80a5f3-239b-4f1f-bb1d-4b9d65dc7b70.png)
   
* 기형적인 형태의 영상이 존재
![image](https://user-images.githubusercontent.com/69246778/128992105-04f033c0-5d08-40e8-b4cf-06926536c7fe.png)
   
* 일부가 가려진 영상이 존재
* 같은 종류여도 형태가 다양함

# 2. CNN과 이미지 Classification
## 2.1. 과거의 이미지 분류
알려진 영상의 특징들을 추출하여 분류를 활용함. 
![image](https://user-images.githubusercontent.com/69246778/128992957-d5b64f4e-8101-431e-93c4-32d876adb463.png)
또는 군집화를 이용해 단순적인 정량화 방법으로 분류. 근데 이 방법들은 결과가 좋지 않음. 이를 보완하는게 CNN.

