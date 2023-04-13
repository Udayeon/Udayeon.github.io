---
layout: post
title: 
description: |
  runtimeerror:cuda error: all cuda-capable devices are busy or unavailable
hide_image: true
tags:
  - deeplearning
published: True
---

# runtimeerror:cuda error: all cuda-capable devices are busy or unavailable
* * *

# 1. 문제상황
평소보다 더 큰 batch size로 학습을 돌린 상황에서 일정 iter만큼 돌아가다가 에러 발생하면서 GPU가 다운(?)되어 버림.   
아래 그림 참조. 0번 GPU에 **ERR!** 발생하면서 학습이 멈추게 됨.   
![image](https://user-images.githubusercontent.com/69246778/231620708-01f2498f-81ae-446b-a2fb-d9d13568bba7.png)   

# 2. 해결책
## 2.1. htop으로 현재 메모리 사용량 확인하기
위의 그림에서 노란색 막대로 표현된 부분이 점점 차고 있는 걸 확인할 수 있다. 이 부분은 **캐시메모리**를 의미하는 부분으로 학습이 진행될수록
캐시 메모리가 차는데 비워지질 않음을 알 수 있다. 
![17967563225342](https://user-images.githubusercontent.com/69246778/231621289-75812c9e-d0b4-41fe-b245-5367294b9512.png)

## 2.2. 캐시메모리 비워주기
아래 명령어를 통해 수동으로 비워줄 수 있음
```
echo 3 > /proc/sys/vm/drop_caches
```
![image](https://user-images.githubusercontent.com/69246778/231625878-14a7ad13-acd4-4f7f-b841-ab1998b30ea8.png)
근데 위와 같은 에러 발생함.   
   
```
sudo -s
```
```
crontab -e
```
![image](https://user-images.githubusercontent.com/69246778/231729064-9a025c30-7121-402c-9f76-051e79d68b4a.png)
```
3 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
6 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
9 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
12 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
15 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
18 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
21 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
24 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
27 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
30 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
33 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
36 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
39 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
42 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
45 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
48 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
51 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
54 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
57 * * * *	sync && echo 3 > /proc/sys/vm/drop_caches
```
배치가 너무 커서 안되길래 3분에 한 번씩 비우는 것으로 수정함.

캐시 비우면서 학습한 결과 reference와 비슷한 성능 보임
![1391](https://user-images.githubusercontent.com/69246778/231729284-0a551ebf-a653-45b0-84fb-591db5580795.png)

