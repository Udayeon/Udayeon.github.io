---
layout: post
title: 
description: |
  voxel grid filter
hide_image: true
tags:
  - projects
published: true
---

# (Ros) Point Cloud Filtering - Voxel , Passthrough
* * *

# 1. Voxel grid filter
![image](https://user-images.githubusercontent.com/69246778/170190024-9935ab51-95a7-4286-96ed-8d5f977fabe2.png)
point 5개가 1개의 Voxel로 표현된다. 그리고 Voxel내 점들의 중심점 하나만 남기고 나머지 점은 제거한다. 즉, 데이터의 크기가 1/5로 줄어든 것. 
voxel의 크기(=leaf size)를 크게 잡으면 더 많은 점을 하나의 데이터로 축소할 수 있다. 대신 물체 표현력도 낮아진다. 
따라서, 물체표현력과 데이터 크기의 트레이드 오프를 잘 고려하여 적절한 voxel size 결정하는 것이 관건이다.

## 1.1. Code
