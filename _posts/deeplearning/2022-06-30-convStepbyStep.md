---
layout: post
title: 
description: |
  Coursera 참고 예제) Convolutional Neural Network 1주차. Convolutional Neural Networks: Step by Step. CNN구현
hide_image: true
tags:
  - deeplearning
published: true
---

# (python) Convolutional Neural Networks: Step by Step - padding
* * *


# 1. Zero padding
![image](https://user-images.githubusercontent.com/69246778/176624846-d6bd5dcb-ef2c-4460-a111-38a1e9c4c6c5.png)
![image](https://user-images.githubusercontent.com/69246778/176632353-a23a1e1c-d52f-4089-823e-18aa171de282.png)
```py
# GRADED FUNCTION: zero_pad

def zero_pad(X, pad):  
    X_pad = np.pad(X, ((0,0),(pad, pad), (pad, pad),(0,0)), 'constant', constant_values=0)
                       #      ,(위,아래),(좌,우)  
    return X_pad
    
#임의의 data 만들기 
# batch size = 2
# height = 4
# width = 4
# channel = 3
np.random.seed(1)
x = np.random.randn(2, 4, 4, 3)

# data x에 zero padding 적용
x_pad = zero_pad(x, 2)  

# 시각화
# imshow : array에 색을 채워서 이미지로 보여줌
fig,axarr= plt.subplots(2,4)
axarr[0][0].set_title('x batch1 whole')
axarr[0][0].imshow(x[0,:,:,0]+x[0,:,:,1]+x[0,:,:,2])   
axarr[0][1].set_title('x batch1 channel1')
axarr[0][1].imshow(x[0,:,:,0])  
axarr[0][2].set_title('x batch1 channel1')
axarr[0][2].imshow(x[0,:,:,1]) 
axarr[0][3].set_title('x batch1 channel1')
axarr[0][3].imshow(x[0,:,:,2])

axarr[1][0].set_title('x_pad batch1 whole')
axarr[1][0].imshow(x_pad[0,:,:,0]+x_pad[0,:,:,1]+x_pad[0,:,:,2])  
axarr[1][1].set_title('x_pad')
axarr[1][1].imshow(x_pad[0,:,:,0])
axarr[1][2].set_title('x_pad')
axarr[1][2].imshow(x_pad[0,:,:,1])
axarr[1][3].set_title('x_pad')
axarr[1][3].imshow(x_pad[0,:,:,2])
```
batch1의 결과   
맨 앞이 전체이미지, 뒤에가 채널 각각.   
![image](https://user-images.githubusercontent.com/69246778/176629480-bb487727-90be-4e9d-b176-aa3f6ed02024.png)   
   
batch2의 결과   
![image](https://user-images.githubusercontent.com/69246778/176629517-409d6722-d262-4189-8b21-28137a08cc32.png)

size 비교, batch랑 채널수는 일정하고 크기만 달라
![image](https://user-images.githubusercontent.com/69246778/176629962-bfa302e2-551f-4d2b-8241-eb8e651a7a8b.png)


