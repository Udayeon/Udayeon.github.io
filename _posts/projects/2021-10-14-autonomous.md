---
layout: post
title: 윈도우에서 딥러닝 개발환경 구축하기 
description: |
  윈도우에서 딥러닝 개발환경 구축하기 
tags:
  - projects
use_math : true
comments : true
author: Udayeon

published: true
---

윈도우에서 딥러닝 개발환경 구축하기 

# 목차
* * *
1. Anaconda
[아나콘다 설치페이지](https://www.anaconda.com/products/individual)
![image](https://user-images.githubusercontent.com/69246778/142382694-721a7f92-af02-4d2c-b1bf-557af4ae4334.png)
윈도우 버전으로 다운로드. 이때, 32비트 버전은 텐서플로가 설치되지 않으므로 64비트를 선택한다.
   
![image](https://user-images.githubusercontent.com/69246778/142382837-5de79d48-3edc-4b46-9028-0d3b8cfb4d97.png)
중간에 뜨는 이 창에서 두 개 모두 체크해준다.
   
설치가 완료되면 cmd창에서 아나콘다가 잘 설치되었는지 확인해본다.
```
C:\Users\유다연> conda
```
이렇게 쳤을 때 에러 없이 다음과 같이 나오면 제대로 설치된 것
![image](https://user-images.githubusercontent.com/69246778/142383326-d3529959-6ae7-4bf2-9a5f-1e71221ab4b4.png)

2.Tensorflow
가상환경을 만들어 준다.
```
(base) C:\Users\유다연> conda create --name tensorflow-gpu python=3.7
```
가상환경 이름은 **tensorflow**, 파이썬은 **3.7**버전 사용
   
가상환경을 만들었으면 활성화한다.
```
(base) C:\Users\유다연> activate tensorflow
```
   
이제 이 가상환경에 텐서플로를 설치해준다.
```
(tensorflow) C:\Users\유다연> conda install tensorflow-gpu=2.0
```
tensorflow 2.0버전으로 설치함   
   
설치확인
```
(tensorflow) C:\Users\유다연>conda list
```
   
다음과 같이 list에 tensorflow가 깔려있는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/69246778/142384119-9abcf9ed-fa52-4774-858d-3612a2e4cd6f.png)
