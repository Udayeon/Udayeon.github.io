---
layout: post
title: Semminar1 - Path Planning paper
description: |
  
tags:
  - autonomous
use_math : true
comments : true
author: Udayeon

published: true
---

# (Review) Path Planning and Tracking for Vehicle Collision Avoidance Based on Model Predictive Control With Multiconstraints

[Path Planning and Tracking for Vehicle Collision Avoidance Based on Model Predictive Control With Multiconstraints](https://ieeexplore.ieee.org/document/7458179)   
**Author** Jie Ji, Amir Khajepour, Wael William Melek, Senior Member, IEEE, and Yanjun Huang     
IEEE TRANSACTIONS ON VEHICULAR TECHNOLOGY, VOL. 66, NO. 2, FEBRUARY 2017
{:.message}

  -  Abstract
  - 1. Introduction
  - 2. Description of collision avoidance system
  - 3. Path Planning for collision avoidance using 3-D virtual dangerous potential field
  - 4. Vehicle mathematical model for path-tracking problem 
  - 5. Design fo multiconstrained model predictive control
  - 6. Simulations of path tracking in different scenarios using carsim and simulink
  - 7. Conclusion

* * *

# Abstract
path planning과 tracking은 자율주행 차량을 충돌로부터 자유롭게 해준다.

# 2. Description of collision avoidance system


# 7.Simulations of path tracking in different scenarios using carsim and simulink
[Section 2]에서 소개했던 famework의 성능을 조사하기 위해 [CarSim]과 [MATLAB Simulink]같은 소프트웨어를 사용하여 MMPC에 의한 수치 시뮬레이션을 시행했다.   
![fig11](https://user-images.githubusercontent.com/69246778/126057072-0a4934ca-a9af-4668-8ff2-28366a614b20.png)
**(Fig 11)** 은 실행한 것을 나타낸 블럭 다이어그램이다.

**NOTE**       
Lateral Position, Yaw rate, Sideslip angle정보와 미리 결정했던 Trajectory 정보를 MPC 컨트롤러가 받아 Front Wheel angle을 조작한다.
계속해서 업데이트 되는 정보를 바탕으로 조향을 결정한다.
{:.message}

이 아키텍쳐에서는 Carsim의 고성능 "big sedan"모델을 사용한다.   
MATLAB Simulink에 내장된 MMPC는 [Section 3]에서 소개한 Planned trajectory를 tracking하기 위해 [closed-loop] 스티어링 조작을 실행하는데 사용된다. 상황이 달라도 같은 컨트롤러를 이용해 차량을 제어할 수 있음에 유념해야 한다.   
이 시뮬레이션 set는 초기 속도로 미리 계획된 경로를 따라가는 **충돌회피 긴급 조작**(직역했음...;)을 나타낸다.  
제어 입력값은 앞 바퀴의 steering angle이고, 목표는 차량과 계획된 경로 간의 편차를 최소화 하면서 계획된 경로를 최대한 가까이 따라가는 것이다.   
이러한 맥락에서, 우리는 미래의 차량이 동물 바위 또는 쓰러진 나무나 나뭇가지 같은 도로 위의 obstacle을 식별하고 완전히 자율적인 조향 시스템으로 원하는 trajectory를 따를 것이라 생각한다. 
[Section A]는 시뮬레이션 시나리오의 상세 내용을 설명하고 [Section B]에서는 시뮬레이션의 가장 중요한 결과와 발견을 설명한다. 

**NOTE**   
도로 위의 obstacle과의 충돌을 긴급히 회피하는 시나리오를 소개한다. 
유념할 것은   
1. 여러 상황에서도 같은 controller를 사용한다는 것
2. control input : 앞바퀴의 steering angle
3. 목표 : 계획된 경로를 추종하는 것
{:.message}

## 7.A. Scenario Description
이번 section에서는 front steering angle에 제약이 있는 일반적인 MPC시스템을 컨트롤러 A라 하고,
front steering angle에 constraint가 input되어 
lateral tracking error, yaw rate, sideslip angle에 대한 constraint를 가진 시스템을 컨트롤러 B라 한다.   
컨트롤러 A의 시뮬레이션 결과를 report하고 컨트롤러 B의 시뮬레이션 결과와 비교한다.
path planning 시뮬레이션과 path tracking 시뮬레이션의 Sample time은 각각 0.2, 0.1이다.

**NOTE**   

Controller A : front steering angle에 constraint가 있는 일반적인 MPC
Controller B : front steering angle에 input constraint가 있고, lateral tracking error,yaw rate, sideslip angle에 state constraint가 있는 MPC
|             |Sample time|
|:------------|:----------|
|Path planning|0.2        |
|Path Tracking|0.1        |
{:.message}

첫 번째 시나리오에서는 선두 차량이 일정속도(15m/s, 10m/s, 0m/s)로 직선 도로를 달린다고 가정한다.   
즉, host 차량의 속도인 20m/s보다 느리기 때문에 충돌하게 된다.     
**(Fig 12)** 에서 보이는 것 처럼, 선두 차량의 현재 위치와 속도에 따라 3차원 가상위험 potential field를 바탕으로 한 path planning 프로그램이 대안적인 trajectory를 생성할 수 있다.   
![fig12](https://user-images.githubusercontent.com/69246778/126023276-4f13fad1-5f32-4a74-8f2e-3fd88ba3ea61.png)   

**NOTE** 
내 차량보다 선두 차량의 속도가 느리면 당연히 충돌하게 됨. 따라서 이를 피해야 하는데 선두 차량의 속도가 어떠냐에 따라 피하는 경로가 다름.   
예를 들어, 앞선 차량의 속도가 느릴수록 더 빨리 꺾어줘야 충돌을 피할 수 있음
{:.message}
   
두 번째 시나리오에서는, 호스트 차량의 초기 속도가 20m/s이고 leading 차량이 host차량보다 17.5m 앞에 위치해 있다. 
(그림13-a)은 미리 정의된 선두 차량의 가속도를 보여준다. 
(그림13-b)는 선두차량의 미리 정의된 Speed command와 command에 대한 Carsim소프트웨어의 속도 응답을 나타낸다.
앞서 언급한 정보를 제안된 3D dangerous potential field에 적용하면 충돌 회피를 위해 계획된 trajectory가 (그림13-c)처럼 생성될 것이다.

## B. Simulation Results
Table 1에 차량 모델의 parameters이 정의되어 있다.
![table1](https://user-images.githubusercontent.com/69246778/126023542-fdddd7c4-2554-47a1-917f-a9d63ab34946.png)

**Scenario 1**  
선두 차량이 10m/s의 속도로 달릴 때, 20m/s의 일정한 속도로 달리고 있는 host 차량은 (그림12)의 빨간색으로 나타낸 궤적을 따라가게 제어되고
차량이 obstacle과 충돌하지 않는지 계속해서 확인하다.   

(그림14)부터 (그림16)은 설계된 MPC기반 경로 추적 컨트롤러A와 컨트롤러B의 성능을 비교하고, trajectory 응답, 차량 응답 등을 각각 나타낸다.
obstacle을 통과하기 전(0s <= t <= 2.5s)에 컨트롤러 양쪽이 좌회전함으로써 obstacle을 피하기 위한 경로를 따라가는 것을 볼 수 있다.
장애물을 통과하고 나면(2.5s <= t <= 6s), 두 개의 컨트롤러는 차량을 오른쪽 차선의 중심선으로 다시 가지고 온다.   
(그림 14)에서, 컨트롤러B는 컨트롤러 A보다 더 나은 path-tracking 성능을 나타낸다.(Planned Trajectory를 더 잘따라가고 tracking error적음) 
컨트롤러A 보다 더 많은 constraint를 가진 컨트롤러B는 (그림 15)에서 처럼, 차량 제어시 더 작은 Steering input command를 사용하고 동시에  더 작은 조향 각속도를 발생시킨다. (확인할 것: 조향 각속도 차이 적을수록 comfort???   
   
컨트롤러B는 또한 컨트롤러A에 비해 더 낮은 yaw rate(ψ)와 side-slip angl(β)을 보인다.   
t가 2.5s보다 큰 경우에서 조작을 하고난 이후의 부분에 대해, (그림16)에서 보이다시피 컨트롤러A는 최대 steering rate로 차량을 다시 선형 영역으로 조향할 수 있고 차를 안정화할 수 있다.   
컨트로러B는 조향각, steering rate, yaw rate, side slip angle등에 제한을 받기 때문에 차량을 항상 선형 영역에 구속해둘 수 있다.   
   
시뮬레이션 결과는 두 컨트롤러가 차량을 계획한 trajectory를 따르게 만들도록 하는 걸 보여준다.   
그러나, 컨트롤러B를 사용하여 더 부드러운 fron steering angle, yaw rate, side slip angle을 얻어 좋은 path trakcing을 할 수 있음에 유념해야한다.
    
 **Scenario 2**
 path planning 프로그램과 MMPC path tracking 컨트롤러의 성능을 평가하기 위해 (그림13)처럼 다양한 속도로 움직이는 obstacle을 생각해보자.
우리는 마찰계수 μ = 0.5으로 설정했다.   
(그림 17)부터 (그림 19)까지는 20m/s로 주행할 때 두 가지 접근법에 대한 시뮬레이션의 차이를 보여준다.
두 개의 컨트롤러는 차량이 계획된 trajector를 따르도록 강제로 steering wheel을 왼쪽으로 꺾으면서 obstacle을 피하려한다.
컨트롤러A는 불안정해지고, 반면 컨트롤러B는 장애물을 피하고도 여전히 계획된 trajectory를 따라 주행한다.   
   
(그림17)은 차량의 trajectory와 tracking error를 보여준다. 빨간선은 컨트롤러A의 결과, 파란선은 컨트롤러B의 결과이고 검정선은 path planning 프로그램이 계산한 계획된 trajectory이다.

컨트롤러A는 충돌 작까지는 차량을 계획된 경로를 따르게 해준다. 그러나 차량이 일단 reference에서 멀리 벗어나게 되면 컨트롤러는 더이상 시스템을 계획된 trajectory로 끌어올 수 없고 그 시스템은 제어할 수 없게 된다.
이러 유형의 동작은 Tire Saturation에 의해 유도되고 이건 (그림 19)처럼 안정성의 threshold를 초과하는 side slip angle과 yaw rate를 유발한다.   
   
우리는 prediction horizon N_p=20, control horizon N_c=5를 확장함으로써 컨트롤러A의 성능을 향상시킬 수 있다. 
그러나, 계산시간이 많이 들고 설명된 실험 platform에서 런타임 오류를 발생시킨다.
   
t=4s보다 큰 테스트의 조작이후, (그림18)에 나타난 것 처럼 컨트롤러A는 planned trajectory를 따라가기 위해 차량을 조종하거나, 최대 steering angle과 rate에서도 안정성을 유지하지도 못한다.   
컨트롤러B는 항상 차량을 선형 영역에 구속하고 컨트롤러A 보다  차량의 tracking 성능을 더 좋게 해준다.

##### [Input command]
##### [Tire saturation]
##### [yaw angle](https://www.researchgate.net/figure/Bicycle-vehicle-model_fig2_346390366)
##### [yaw_rate](https://blog.naver.com/jjz0426/221135413776)

