---
layout: post
title: 
description: |
  (Error) CUDA out of memory
hide_image: true
tags:
  - etc
published: true
---

# (Error) CUDA out of memory
* * *

# 1. Batch Size를 바꿔준다
Batch size를 담고 있는 파일로 이동해서 **gpu~**관련 부분의 숫자 크기를 작게 조정한다. 근데 아무리 작게 해도 계속 **out of memory**가 발생하거나, 배치사이즈가 너무 
작아서 한 번 훈련하는데 9일 이상 걸리게 될 수도 있음. 


# 2. GPU 비워주기
트레이닝을 하다보면, 중간에 끊기더라도 GPU가 비워지지 않기도 한다. 따라서 훈련 동안
쓴 GPU를 비워주면 됨.   
@Terminal   
아래 명령어를 실행하면 현재 실행 중인 GPU의 사용량과, 돌아가는 프로그램을 확인할 수 있음
```
nvidia-smi
```
   
아래와 같이 특정 PID를 입력해서 프로그램을 종료할 수 있음. 그러나 현재 보고 있는
디스플레이가 종료되거나, 별 용량을 쓰지도 않는 사소한 프로그램만 종료될 뿐 효과가
없음.
```
sudo kill -9 [PID]
```   
   
그럴땐 아래와 같이 **Python**관련 작업을 모두 종료해주면 됨.
```
sudo pkill -9 python
```
   
그리고 다시 **nvidia-smi**해보면 GPU용량이 비워져있고 트레이닝도 다시 잘 됨.
