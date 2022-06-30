---
layout: post
title: 
description: |
  numpy array형식
hide_image: true
tags:
  - programming
published: true
---

# (python) numpy array
* * *

# 1. array
## 1.1. case1
```py
case1 = np.random.randn(3,2)
```
![image](https://user-images.githubusercontent.com/69246778/176597794-b3c68282-ae56-4c13-aa63-e449013cc269.png)
2개씩 * 3묶음

## 1.2. case2
```py
case2 = np.random.randn(2,3,4)
```
![image](https://user-images.githubusercontent.com/69246778/176597969-92a82070-78d7-4045-8c29-a0b30684df53.png)
(4개씩 * 3묶음) * 2개

## 1.3. case3
```py
case3 = np.random.randn(3,4,2,2)
```
![image](https://user-images.githubusercontent.com/69246778/176598571-d05809a7-9976-4886-91b0-03929bd65cd8.png)
((2개씩 * 2묶음) * 4개 ) * 3개

