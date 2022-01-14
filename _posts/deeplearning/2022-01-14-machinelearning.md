---
layout: post
title: 
description: |
  게시물 설명 간략히
hide_image: true
tags:
  - deeplearning
published: true
---

# 게시물 제목
* * *

# 1. 본문제목
구체적인 인공 신경망 중에 하나로 다음과 같은 구조를 가짐.   
![image](https://user-images.githubusercontent.com/69246778/129148386-6b401f0e-3821-4cdf-9088-4f4eb070cd77.png)

## 1.1. 소제목
내용내용

## 1.2. 소제목
어쩔저쩔

# 2. 
MLP는 모든 노드들이 input부터 output까지 다 연결되어 있는 완전 연결을 가지고 있어 복잡도가 높아 학습이 매우 어렵거나 느리고 
Over-fitting문제가 발생할 수 있음. 반면, CNN은 부분적인 연결 즉, 희소연결을 통해 구조적인 복잡도를 낮추었고 입력의 지역성(locality)기반의
특징 추출. 

## 2.1.소제목
격자 구조 형태(영상, 음성처럼 정량화 된 구조의 특징에 적용되는 입력)의 데이터 처리에 효율적. 기본 단위는 완전연결이 아니라 부분연결임.
Receptive Field(수용장)에 의해서 부분, 부분을 이해하면서 그 안에서 특징을 뽑는 형태. 인간의 시각과 유사하고 CNN자체는 가변적인 크기의 
입력 처리가 가능. 완전 연결을 가지는 MLT와 달리 부분적인 입력이 가능하기 때문. 만약, 완전 연결이 256x256 input과 10개의 output
