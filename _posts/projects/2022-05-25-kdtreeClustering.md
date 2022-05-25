---
layout: post
title: 
description: |
  ros튜토리얼 - kdtree , clustering
hide_image: true
tags:
  - projects
published: true
---

# (Ros) Point Cloud Filtering - kdtree , clustering
* * *

# 1. kdtree
![image](https://user-images.githubusercontent.com/69246778/170198765-297842ed-64d0-4fa3-b99d-f1ae48f633eb.png)
![image](https://user-images.githubusercontent.com/69246778/170199064-9f947efa-7f20-4454-9d64-693b592b7e2d.png)
K차원으로 공간상의 점들을 정리하는 자료구조.   
   
Point cloud에서는 두 가지 방법이 있다.
* 초기에 점의 갯수를 설정하고 그 갯수가 만족될 때까지 영역을 탐색하는 방법.
* 탐색 시작점을 기준으로 특정 반경 내에 있는 모든 포인트를 탐색하는 방법.
원하는 영역만 segmentation할 수 있다. 

 
## 1.1. Code
