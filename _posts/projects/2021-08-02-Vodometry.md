---
layout: post
title: 
description: |
  간단한 mono VO를 실행해보고 성능 향상을 위해 파라미터를 바꿔가며 변화를 관찰
hide_image: true
tags:
  - projects
published: true
---


# (cpp) 간단한 Visual Odometry
* * *
[blog post](https://avisingh599.github.io/vision/monocular-vo/)
[dataset](http://www.cvlibs.net/datasets/kitti/eval_odometry.php)
[code](https://github.com/avisingh599/mono-vo)

# 1. Setting
* git에서 mono-vo file 다운받음. edu-slam으로 저장했음.   
![image](https://user-images.githubusercontent.com/69246778/128284681-c5ce865e-13ce-4307-9c3e-f999f7073a9b.png)   
   
* compile 해줌
![image](https://user-images.githubusercontent.com/69246778/128284910-bcf3ac07-1f41-44af-905f-72781ba638ea.png)
![image](https://user-images.githubusercontent.com/69246778/128285083-402ffd8b-def8-431b-9926-dd02150c859b.png)
build해주고 기존에 받아둔 KITTI data를 dataset file에 넣어줌(소스코드에 있던 파일경로대로 걍 따라감)
![image](https://user-images.githubusercontent.com/69246778/128285198-a10f2d11-db08-4d7f-8197-4cd3a3c436a5.png)
user name 내이름인 carrot으로 다 변경해주는 거 잊지말기

* 실행
![image](https://user-images.githubusercontent.com/69246778/128285345-b23685f2-260c-47db-85e7-a530bb714938.png)
build 디렉토리에서 ./edu-slam 치면 실행됨.

* 결과
fast threshold =100   
![image](https://user-images.githubusercontent.com/69246778/128450583-bcfe6743-0bd8-42f2-b407-14bb89a75f30.png)   
   
![image](https://user-images.githubusercontent.com/69246778/128450883-5e6fc7f5-e8fa-40e2-b529-e83be145ae17.png)   
![image](https://user-images.githubusercontent.com/69246778/128451090-6ce22707-0539-4db0-b562-a9bd7830399a.png)
![image](https://user-images.githubusercontent.com/69246778/128451111-4c82ebdc-f2e3-41a0-bef3-36ea6b4b0986.png)

실행하면 속도가 개빠름 정신없음. 동시에 Trajectory 출력됨   



# 2. Discussion
Calibration parameter나 dataset, threshold를 자유롭게 수정하여 결과가 좋도록 만들기
mono vo는 전역 최적화가 없음. 취약점과 그 원인 파악.
ORB SLAM으로 이를 어떻게 극복하는지 고찰
{:.message}

## 2.1. Threshold
![image](https://user-images.githubusercontent.com/69246778/128446933-24c85a5a-8075-4bbc-a5a6-1b873136b10a.png)   
feature.h 파일에서 fast threshold를 먼저 수정해보자   

* Threshold = 100
![image](https://user-images.githubusercontent.com/69246778/128454653-59e32f18-e14a-4814-83f4-fe2e93cebf1c.png)

* Threshold = 95
![image](https://user-images.githubusercontent.com/69246778/128454996-c83fbaa7-3c37-49cb-822a-8282e74bc604.png)

* Threshold = 105
![image](https://user-images.githubusercontent.com/69246778/128455160-58b68660-07d8-45fb-897d-a5e97fbd97c0.png)


