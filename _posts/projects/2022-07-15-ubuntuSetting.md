---
layout: post
title: 
description: |
  우분투에서 딥러닝 개발환경 구축하기
hide_image: true
tags:
  - projects
published: true
---

# 우분투에서 딥러닝 개발 환경 구축하기 - Swin Transformer 실행
* * *

# 1. pytorch Docker 설치
```
docker run --gpus all -it --rm nvcr.io/nvidia/pytorch:21.05-py3
```
swin transformer는 nvcr>=21.05

![image](https://user-images.githubusercontent.com/69246778/179339578-3e07955a-e2b9-4cdb-ab0f-75796397800d.png)
위와 같이 **permission denied**가 발생할 경우 다음의 포스트 참조
[docker 설치 후 /var/run/docker.sock의 permission denied 발생하는 경우](https://github.com/occidere/TIL/issues/116)
```
sudo chmod 666 /var/run/docker.sock
```
위의 명령어로 파일 권한 변경 후 다시 docker run~하면 설치 된다. 
![image](https://user-images.githubusercontent.com/69246778/179339662-760dae3d-5a10-444b-b0cd-f92a2e48dcbc.png)


## 1.1. 소제목
내용내용

## 1.2. 소제목
어쩔저쩔
