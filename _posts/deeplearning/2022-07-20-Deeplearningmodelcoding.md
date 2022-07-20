---
layout: post
title: 
description: |
  Deep Learning Model 구현
hide_image: true
tags:
  - deeplearning
published: True
---

# 딥러닝 모델 구현 방법
* * *

# 1. 전체적인 Architecture
![image](https://user-images.githubusercontent.com/69246778/179893632-82cca084-8d7e-437b-b30b-869fab098fd8.png)
swin transformer의 경우, 크게 4개의 stage로 구성되어 있다. 각 stage별로 swin transformer block이 2개 혹은
6개 들어있는데 이 block은 기본적으로 2개가 연속적으로 붙어 있다. 첫 block은 W-MSA를 하고 둘째 block은
sW-MSA를 하는 것이 차이점이다. 예를 들어, stage3의 경우 swin transformer block이 6개 들어있는데 이는
W-MSA를 하는 블럭과 SW-MSA를 하는 블럭이 연속되어 3쌍 있는 셈.   
   
전체적인 플로우는, 먼저 3채널의 input 이미지를 Patch로 나누어 첫 번째 stage에 넣어줌. 각 Patch에 대해
Linear Embedding을 진행하고 이를 Swin block에 넣어준다. 한 stage가 끝나면 다음 stage로 넘어가는데
다음 stage의 블럭에 넣기 전에 잘려있던 patch를 Merging해준다.


# 2. layer
## 2.0. Input layer
### Input Image Size : H(height) * W(Width) * C(Channel)
### Patch Size : p_H(patch height) * p_W(patch width)

## 2.1. Stage1
### Input Image를 잘게 쪼갠 Patch들이 input으로 들어옴.
![image](https://user-images.githubusercontent.com/69246778/179895906-1554ad01-8c32-4345-b216-db026c162f6a.png)
patch들을 Window의 고정된 크기만큼 묶어준다.
만약 patch가 56 * 56 개 있고, window 크기가 7이면 patch 7 * 7 개를 하나의 window로 묶어주는 것.   
그럼 총 8 * 8개의 window 묶음이 생기는 셈. 만약 patch가 28 * 28 개 있으면, 4 * 4개의 window가 생김.    
이 window에 대해 첫 block에서 MSA를 진행한다. 둘째 block에서는 window를 약간 shift해서 window내에 포함되는 patch가 달라지도록 한 후에 MSA를 진행(SW-MSA)  
shift크기는 window size의 절반으로 설정한다. window size는 절대적인 크기를 말하는게 아니라 그 안에 몇 개의 patch가 포함될 수 있는지를 말하는 것임.   
따라서, patch size가 크면 window의 절대적인 크기 자체도 커지게 됨. 큰 크기의 patch 7개 넣어놓은 window랑 작은 크기의 patch 7개 넣어놓은 window는
다른 크기를 가짐. 
![image](https://user-images.githubusercontent.com/69246778/179896939-476b270e-b26b-4888-b0cf-1f34d88daaca.png)
![image](https://user-images.githubusercontent.com/69246778/179896995-98629959-8b4f-4bc8-936a-3cc637b362e6.png)





## 2.2. Stage2


# 3. Module
