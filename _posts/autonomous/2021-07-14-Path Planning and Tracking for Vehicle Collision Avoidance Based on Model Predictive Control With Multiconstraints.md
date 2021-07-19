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
    - 1.Introduction
    - 2.Description of collision avoidance system
    - 3.Path Planning for collision avoidance using 3-D virtual dangerous potential field
    - 4.Vehicle mathematical model for path-tracking problem 
    - 5.Design fo multiconstrained model predictive control
    - 6.Simulations of path tracking in different scenarios using carsim and simulink
    - 7.Conclusion

* * *

# Abstract
* * *

# 1.Introduction
* * *

# 2. Description of collision avoidance system
* * *

# 3. Path Planning for collision avoidance using 3-D virtual dangerous potential field
* * *

# 4. Vehicle mathematical model for path-tracking problem 
* * *
Path tracking 문제는 차량 모델링에 의존한다. 왜냐하면 모델링은 MMPC법 설계에 필요한 요소이기 때문이다. 본 논문에서 사용하는 모델은 차량의 
운동학적 및 동적 측면을 고려해야 한다. 여기서, 우리는 충돌 회피 시스템 개발에 사용되는 차량의 확장적 수학 모델을 제안한다. [Section 4-A]에
서는 lateral 및 yaw dynamic을 고려한 차량 dynamic 모델을 개발하고 [Section 4-B]에서는 MMPC개발에 사용되는 이산적인 상태 공간 차량 모델을 
소개한다.   

```
📝NOTE
MMPC법을 설계하기 위해선 차량 모델링이 필요하다.
이 논문에선 차량의 운동학적 및 동적 측면을 고려한 모델이 필요하다.
그래서 충돌 회피 시스템 개발에 사용되는 augmented mathematical model을 제안한다.
section A : lateral 및 yaw를 고려한 모델 개발
section B : MMPC개발에 사용되는 discrete state-space 모델 소개
```

## 4.A. Vehicle Dynamic Model for Path Tracking
```
📝NOTE
lateral 및 yaw를 고려한 모델 개발
```

이 Section에서는 control design에 사용될 차량과 타이어의 모델링에 대해 설명한다. path-tracking 문제에서 차량 모델링에서의 가정은
다음과 같다.
- longitudinal 속도는 일정하다.
- 앞축과 뒤축에서 왼쪽, 오른쪽 바퀴는 single wheel 하나로 묶는다.
- 서스펜션 운동과 미끄러짐, 공기역학의 효과는 무시한다.
이러한 가정들을 통해 그린 일반 차량의 선형 동적 모델은 **(Fig 9)** 와 같다. 이는 뉴턴의 법칙에 따라 구해진 것이다.   

![fig9](https://user-images.githubusercontent.com/69246778/126086178-1ff13692-8dd8-4b54-9d24-7c62f6a8093b.png)

차체의 sideslip angle β 와 차체의 yaw rate ψ˙은 상태 변수로 간주되며, 차량의 lateral dynamics는 다음과 같이 쓸 수 있다.   
![image](https://user-images.githubusercontent.com/69246778/126086210-ffad5d77-0948-426c-b1fe-ac9533ace72f.png)

![I_z](https://user-images.githubusercontent.com/69246778/126086292-3c1d7e07-7d66-4026-a022-a1a323ba22a7.gif)는 yaw 축에 관한 차량의 Inertia이다. ![l_f](https://user-images.githubusercontent.com/69246778/126086315-0d6a8c4b-51d8-4272-9cee-2c468bb66ab5.gif)와 ![l_r](https://user-images.githubusercontent.com/69246778/126086319-48a6cc62-a10c-4db6-a21c-4df7d3a93945.gif)은 각각 Center of gravity(CG)로 부터 앞, 뒤 바퀴 간의 거리이다. 
{:.read}

```
📝NOTE
Fig 9       : 위의 세가지 가정을 전제로 하여 그린 차량의 linear dynamic model

식12, 식13  : 차량의 lateral dynamics
  β  : sideslip angle  (state 변수) 
  ψ˙ : yaw rate        (state 변수)
  Iz : yaw축에 관한 차량의 Inertia
```

코너링하는 타이어의 힘에 대한 여러가지 모델이 존재한다. tire slip angle이 작을 때, lateral tire force는 tire slip angle의 선형함수로
근사된다. 앞바퀴, 뒷바퀴의 tire force ![F_xf](https://user-images.githubusercontent.com/69246778/126086837-faa20b68-3bfd-4353-b8a1-1c5cdfedca36.gif),![F_xr](https://user-images.githubusercontent.com/69246778/126086843-5aac3490-0e79-4342-8630-3974e18a1a9c.gif)
 와 tire slip angle ![alpha_f](https://user-images.githubusercontent.com/69246778/126086850-8b908845-73e5-47f6-ab04-45ea83e9c893.gif),![alpha_r](https://user-images.githubusercontent.com/69246778/126086851-08fb8075-dd8c-4ad4-8c8e-725271a905ef.gif)은 다음과 같이 정의된다.   

![image](https://user-images.githubusercontent.com/69246778/126086712-db4d1cef-0d99-44ef-a4cf-9d9ecaf02e96.png)

여기서 δ는 앞바퀴의 조향각이다. ![C_f](https://user-images.githubusercontent.com/69246778/126086919-7f18cfea-6f14-487a-a6c4-eda9f6ddd794.gif)와 ![C_r](https://user-images.githubusercontent.com/69246778/126086927-41a8e60f-78b9-405d-8e37-cf9822788db0.gif)은 각각 앞바퀴,뒷바퀴의 선형화된 cornering stiffness를 나타낸다.  

```
📝NOTE
식 14, 식 15 : tire force를 tire side slip angle에 선형 근사한 식
F     : tire force
alpha : tire slip angle
delta : front-wheel streeing angle
C     : cornering stiffness
```

식 (14),(15)를 식(12),(13)에 대입해서 구한 다음의 식은 lateral 및 yaw 의 동력학을 다루는 식이 된다.   

![Page3](https://user-images.githubusercontent.com/69246778/126087981-24205fe5-fa21-40e0-b647-d104ea186056.jpg)   
![image](https://user-images.githubusercontent.com/69246778/126086686-cc2a9733-6421-490d-b426-9db65bac174c.png)   
    
```
📝NOTE
식 16, 식 17 : 차량 모델의 lateral and yaw dynamics
```

## 4.B. Discrete linear vehicle model for MPC
```
📝NOTE
Section A에서 lateral 및 yaw를 다루는 식을 유도했고
이 식으로부터 MMPC최적화를 위한 discrete state-space식을 유도한다.
```
여기서, 우리는 MMPC최적화를 위해 이산 상태-공간 차량 모델을, 이전 section에서 얻은 수학적 모델로부터 유도한다. 새로운 차량 모델에서, 상태
공간 벡터는 차량 CG의 lateral 위치 ![X_v](https://user-images.githubusercontent.com/69246778/126088231-a9e77bcb-958a-4935-b150-c6cb4eef7b8a.gif), 차량의 side slip angle β, yaw angle ψ, yaw rate ψ'로 구성된다. input은 앞 바퀴의 steering angle δ로 주어진다. 이런 정의에 의해,  state-space 벡터는 다음과 같다.   

![image](https://user-images.githubusercontent.com/69246778/126088279-dfc09e92-3595-4fbd-9369-2092f0243825.png)

상태 방정식은 이전 section에서 유도된 **(식16)** 과 **(식17)** 에 기반해 다음과 같이 쓸 수 있다.   

![image](https://user-images.githubusercontent.com/69246778/126088300-81dca464-3706-4f6d-904e-dbbac2357633.png)

이 때 ![A_c](https://user-images.githubusercontent.com/69246778/126088351-bf48ad0e-da72-441a-9bf2-cdafbf14ad49.gif), ![B_c](https://user-images.githubusercontent.com/69246778/126088354-52df69cc-162c-49e1-b54a-611bcb27faef.gif), ![C_c](https://user-images.githubusercontent.com/69246778/126088357-e7ddd61b-bb42-4aac-bbe0-17bef4491b26.gif)는 다음과 같다.

![image](https://user-images.githubusercontent.com/69246778/126088319-7515207b-0ea8-4620-8604-7a8342efc3a3.png)

```
📝NOTE
Xc : lateral position, sideslip angle, yaw angle, yaw rate로 구성된 state-space 벡터
이 상태 벡터와 앞서 구한 식16,식17에 기반해 상태방정식 식19,식20 을 쓸 수 있음.
```

앞에서 언급한 차량 모델은 선형화 된 연속시간 및 단일입력, 다중출력 시스템이다. 그러나, 제어될 시스템은 일반적으로 MPC [literatue 29]의
discrete state-space model에 의해 모델링된다. 따라서, **(식 19)** 와 **(식 20)** 은 **(식 22)** 을 얻기 위해 discrete state-space 모
델로 변환되어야 한다.   

![image](https://user-images.githubusercontent.com/69246778/126089046-275d70a2-804e-486c-a610-25848954305c.png)

여기서 ![A_d](https://user-images.githubusercontent.com/69246778/126089152-4ce2a816-f125-4d67-98fb-2451e52027b0.gif)와 ![B_d]
(https://user-images.githubusercontent.com/69246778/126089156-5b62ca79-080c-4bbe-a282-8b8470b20dc3.gif)는 각각 discrete state-
space equation을 설명하기 위한 state 매트릭스와 control 매트릭스를 나타내고 이는 다음과 같이 [오일러 method]로 계산될 수 있다.

![image](https://user-images.githubusercontent.com/69246778/126089228-edaea7ed-59cc-4225-8b09-447b314181c7.png)

이 때, ΔT 는 discrete state-space 모델의 샘플링 간격을 의미한다.  
```
📝NOTE
식 19, 식 20은 선형화, 연속시간, 단일입력, 다중출력 시스템이다.
그러나 제어될 시스템은 discrete state-space model이므로 변환해주어야 한다.
그럼 식22를 구할 수 있는데 이를 구성하는 계수인 Ad(State),Bd(control)는 오일러 방식으로 구할 수 있다.
Discrete state-space model : 29번 문헌 174page
```

lateral 위치, sideslip angle, 그리고 yaw rate는 다음의 식을 사용하여 output으로 정의할 수 있다.   

![image](https://user-images.githubusercontent.com/69246778/126089544-36931240-e6ac-4436-a116-e8f05c18251b.png)


여기서
![Y_d(k)](https://user-images.githubusercontent.com/69246778/126089692-e0250dda-0841-40cb-a4e9-bb891d4b0ae9.gif)와
![C_d](https://user-images.githubusercontent.com/69246778/126089732-4c8f858d-3453-4154-ba30-81904354b433.gif)는 다음과 같다.   

![image](https://user-images.githubusercontent.com/69246778/126089764-bafc6d47-0d61-46be-b8a9-35a98a8d7897.png)

```
📝NOTE
output으로 lateral,sideslip angle, yaw rate를 얻기 위해 식25를 사용한다.
output을 나타내는 벡터 Yd는 Xv(lateral),β(sideslip angle),Ψ(yaw rate) 로 구성되어 있다.
우항의 Xd(k)는 식22에서 구했고 Cd는 Cc와 같다.
```

MMPC를 사용한 Path tracking에서 [plant]변수에 의한 하드웨어 constraint와 output에 의한 소프트웨어 constraint에 의해 constrained 
control problem 을 실시간 최적화 문제로 공식화하는 것은 일반적이다. discrete state-space 모델인 **(식22)**와 **(식25)** 를 통합해 
단일화된 모델로 확장시킬 수 있다.

```
📝NOTE
하드웨어, 소프트웨어 모두 constraint이 있으므로 
constrain된 제어 문제를 실시간 최적화 문제로 생각해 푸는 것은 당연..
식 22 : discrete state-space model를 계산하는 식
식 23 : output출력하는 식
```

상태변수와 제어변수의 차이를 다음과 같이 표시해보자.

![image](https://user-images.githubusercontent.com/69246778/126090282-274c43f9-69bc-4853-a62d-62081b0702da.png)

```
📝NOTE

```


**(식 27)~(식 29)** 를 **(식 22)** 와 **(식 25)** 에 대입하면 다음과 같이 쓸 수 있고, 이는 변수 Xd(k)와 u(k)의 증분을 갖는 discrete
state-space model이다.   
   
state-space model과 output방정식에 대한 input은 delta_u(k)이다. delta_Xd(k)를 output인 Y(k)와 연결하기 위해 새로운 상태 변수 벡터를
다음과 같이 설정한다.
   
**(식 32)** 를 **(식 30)** 과 **(식 31)** 에 결합하면 다음과 같은 state-space model이 만들어진다.

여기서 (Aa,Ba,Ca)는 augmented model(증강모형)이라 불리고 다음과 같이 쓸 수 있다.


# 5. Design fo multiconstrained model predictive control
* * *
path tracking 은 차량 역학과 운동학에서 발생한 constraint에 대한 예측제어 문제로 제기될 수 있다. 여기에 제시된 분석은 [29]에 기초하지만
차량 충돌회피 application에 적합하도록 조정된다. 

## 5.A. Prediction of State and Output Variables
path tracking을 위한 MPC의 디자인에 있어서 각 시간마다 차랴의 미래 행동을 예측하는 것으 중요한 단계이다. 이 미래 예측은 특정한
prediction horizon 내에서 control input을 결정해주고 이 미래 상태에 기초하여, 최적화된 control input을 계산하기 위해 성능지수가 최소화
된다.

```
📝NOTE
performance index가 최소화 된다는게 뭐임?
```

여기에 우리가 현재 시간 k를 가정했고 이는 항상 양수이다. prediction horizon은 optimization window의 길이인 Np=10이고,
control horizon Nc=5이다. 상태 변수 벡터 Xa(k)는 현재의 plant 정보를 제공하고, 이는 측정을 통해 사용할 수 있다.   
주어진 정보 Xa(k)를 통해 Np단계에 대해 다음과 같이 미래 상태 변수를 예측할 수 있다. 

여기서 Xa(k+m)은 현재의 plant 정보 Xa(k)를 통해 k+m에어싀 예측된 상태 변수이다.   
우리는 ΔUm으로 현재 관측상태에 대해 시간 k에서 계산한 미래 input 증분의 순서를 다음과 같이 나타낸다.





```
📝NOTE   
plant information??
future increment??
```

# 6. Simulations of path tracking in different scenarios using carsim and simulink
* * *
[Section 2]에서 소개했던 famework의 성능을 조사하기 위해 [CarSim]과 [MATLAB Simulink]같은 소프트웨어를 사용하여 MMPC에 의한 수치 시뮬레이션을 시행했다.
**(Fig 11)** 은 실행한 것을 나타낸 블럭 다이어그램이다.   

![fig11](https://user-images.githubusercontent.com/69246778/126057072-0a4934ca-a9af-4668-8ff2-28366a614b20.png)   

```
📝NOTE      
Lateral Position, Yaw rate, Sideslip angle정보와 미리 결정했던 Trajectory 정보를   
MPC 컨트롤러가 받아 Front Wheel angle을 조작한다.   
계속해서 업데이트 되는 정보를 바탕으로 조향을 결정한다.
```

이 아키텍쳐에서는 Carsim의 고성능 "big sedan"모델을 사용한다. MATLAB Simulink에 내장된 MMPC는 [Section 3]에서 소개한 Planned trajectory를 tracking하기 
위해 [closed-loop] 스티어링 조작을 실행하는데 사용된다. 상황이 달라도 같은 컨트롤러를 이용해 차량을 제어할 수 있음에 유념해야 한다. 이 시뮬레이션 set는 초기 
속도로 미리 계획된 경로를 따라가는 **충돌회피 긴급 조작**(직역했음...;)을 나타낸다. 제어 입력값은 앞 바퀴의 steering angle이고, 목표는 차량과 계획된 경로 
간의 편차를 최소화 하면서 계획된 경로를 최대한 가까이 따라가는 것이다. 이러한 맥락에서, 우리는 미래의 차량이 동물 바위 또는 쓰러진 나무나 나뭇가지 같은 도로 
위의 obstacle을 식별하고 완전히 자율적인 조향 시스템으로 원하는 trajectory를 따를 것이라 생각한다.   
[Section A]는 시뮬레이션 시나리오의 상세 내용을 설명하고 [Section B]에서는 시뮬레이션의 가장 중요한 결과와 발견을 설명한다. 

```
📝NOTE   
도로 위의 obstacle과의 충돌을 긴급히 회피하는 시나리오를 소개한다. 유념할 것은
- 여러 상황에서도 같은 controller를 사용한다는 것
- control input : 앞바퀴의 steering angle
- 목표 : 계획된 경로를 추종하는 것
```

## 7.A. Scenario Description
이번 section에서는 front steering angle에 제약이 있는 일반적인 MPC시스템을 컨트롤러 A라 하고, front steering angle에 constraint가 input되어 lateral
tracking error, yaw rate, sideslip angle에 대한 constraint를 가진 시스템을 컨트롤러 B라 한다. 컨트롤러 A의 시뮬레이션 결과를 report하고 컨트롤러 B의 시
뮬레이션 결과와 비교한다. path planning 시뮬레이션과 path tracking 시뮬레이션의 Sample time은 각각 0.2, 0.1이다.

```
📝NOTE      
- Controller A : front steering angle에 constraint가 있는 일반적인 MPC   
- Controller B : front steering angle에 input constraint가 있고,   
                 lateral tracking error,yaw rate, sideslip angle에 state constraint가 있는 MPC  
- Sample time : Path planning 0.2s / Path Tracking 0.1s 
```

**첫 번째 시나리오**에서는 선두 차량이 일정속도(15m/s, 10m/s, 0m/s)로 직선 도로를 달린다고 가정한다. 즉, host 차량의 속도인 20m/s보다 느리기 때문에 충돌하게 된
다. **(Fig 12)** 에서 보이는 것 처럼, 선두 차량의 현재 위치와 속도에 따라 3차원 가상위험 potential field를 바탕으로 한 path planning 프로그램이 대안적인 
trajectory를 생성할 수 있다.   

![fig12](https://user-images.githubusercontent.com/69246778/126023276-4f13fad1-5f32-4a74-8f2e-3fd88ba3ea61.png)   

```
📝NOTE   
내 차량보다 선두 차량의 속도가 느리면 당연히 충돌하게 됨.   
따라서 이를 피해야 하는데 선두 차량의 속도가 어떠냐에 따라 피하는 경로가 다름.   
예를 들어, 앞선 차량의 속도가 느릴수록 더 빨리 꺾어줘야 충돌을 피할 수 있음
```

**두 번째 시나리오**에서는, host 차량의 초기 속도가 20m/s이고 선두차량이 host차량보다 17.5m 앞에 위치해 있다.
**(Fig 13-a)** 은 미리 정의된 선두 차량의 가속도를 보여준다. 
**(Fig 13-b)** 는 선두차량의 미리 정의된 Speed command와 command에 대한 Carsim소프트웨어의 속도 응답을 나타낸다.
앞서 언급한 정보를 제안된 **3D dangerous potential field**에 적용하면 충돌 회피를 위해 계획된 trajectory가 **(Fig 13-c)** 처럼 생성될 것이다.   

![fig13](https://user-images.githubusercontent.com/69246778/126058726-b6af4124-6463-4298-aa25-eebadbc5ccd9.png)   

```
📝NOTE   
선두차량의 초기속도 20m/s인데  2s부터 4s에 감속하여 10m/s까지 내려갔다가 다시 가속하여 30m/s로 주행한다.   
- time 0s ~ 2s : obstacle 만나기 전 직선 주행( 0~40m 구간)  
- time 2s ~ 4s : obstacle 회피 위해 감속하며 좌측 차선으로 ( 40m~70m 구간 )
- time 4s ~ 6s : obstacle 회피 이후 가속하여 우측 차선으로 돌아옴 ( 70m~100m 구간)
- time 6s~   : 초기 속도보다 빨라진 속도로 직선주행 ( 100m이후 구간)
```

## 7.B. Simulation Results

```
📝NOTE   
위에서 정의한 시나리오 설명을 기반으로 실험한 결과
```

Table 1에 차량 모델의 parameters이 정의되어 있다.   

![table1](https://user-images.githubusercontent.com/69246778/126023542-fdddd7c4-2554-47a1-917f-a9d63ab34946.png)   

**Scenario 1**   
선두 차량이 10m/s의 속도로 달릴 때, 20m/s의 일정한 속도로 달리고 있는 host 차량은 **(Fig 12)** 의 빨간색으로 나타낸 궤적을 따라가게 제어되고 
차량이 obstacle과 충돌하지 않는지 계속해서 확인한다.   

![fig12](https://user-images.githubusercontent.com/69246778/126061123-10e8f42c-65e7-41ee-ba39-09f3177a6933.png)   

```
📝NOTE   
왜 빨간색???빨간색은 V=0일 때 아닌가
```

**(Fig 14)** 부터 **(Fig 16)** 은 설계된 MPC기반 경로 추적 컨트롤러A와 컨트롤러B의 성능을 비교하고, trajectory 응답, 차량 응답 등을 각각 나타낸다.   
- time 0s ~ 2.5s : obstacle을 통과하기 전, 두 컨트롤러 좌회전함으로써 obstacle을 피하기 위한 경로를 따라가는 것을 볼 수 있다.   
- time 2.5s ~ 6s : 장애물을 통과하고 나면 두 개의 컨트롤러는 차량을 오른쪽 차선의 중심선으로 다시 가지고 온다.   
  
![fig14](https://user-images.githubusercontent.com/69246778/126059526-eb4d88a1-b9f1-4a57-bb8b-302533c9bd79.png)   

**(Fig 14)** 에서, 컨트롤러B는 컨트롤러 A보다 더 나은 path-tracking 성능을 나타낸다. 

```
📝NOTE   
Planned Trajectory를 더 잘따라가고 tracking error적음
```

![fig15](https://user-images.githubusercontent.com/69246778/126060358-1d42c1b4-9949-4cbf-9bb9-dff891ae08e3.png)   

컨트롤러A 보다 더 많은 constraint를 가진 컨트롤러B는 **(Fig 15)** 에서 처럼, 차량 제어시 더 작은 Steering input command를 사용하고 동시에  더 작은 조향 
각속도를 발생시킨다.

```
📝NOTE   
컨트롤러B는 A보다 많은 Constraint를 가지고 있어서 경로 추적도 더 잘하고   
tarcking error도 적고 조향각속도도 크게 발생하지 않아서 흔들림이 덜함
```

컨트롤러B는 또한 컨트롤러A에 비해 더 낮은 [yaw rate(ψ)]와 [side-slip angl(β)]을 보인다. 조작을 하고난 이후(obstacle을 pass한 2.5s 이후)에, **(Fig 16)**
에서 보이다시피 컨트롤러A는 최대 [steering rate]로 차량을 다시 선형 영역으로 조종하고 차량을 안정화한다. 컨트롤러B는 steering angle, steering rate, yaw 
rate, side slip angle등에 constraint 덕에 차량을 항상 선형 영역에 구속해둘 수 있다.   

![fig16](https://user-images.githubusercontent.com/69246778/126059803-66489ac0-c8fd-4ab5-9469-9a8a4dcc5fcd.png)   
   
시뮬레이션 결과는 두 컨트롤러가 차량을 계획한 trajectory를 따르도록 하는 걸 보여준다.   
그러나, 컨트롤러B를 사용하여 더 부드러운 front steering angle, yaw rate, side slip angle와 함께 좋은 path trakcing을 할 수 있음에 유념해야한다.

```
📝NOTE   
컨트롤러A는 차량을 선형 영역에 두기 위해 큰 steering rate가 필요.   
yaw rate, sideslip angle도 크게 발생함.   
반면, 컨트롤러B는 A보다 많은 Constraint를 가지고 있어서 차량을 항상 선형 영역에 구속시킬 수 있음. 
```

**Scenario 2**
path planning 프로그램과 MMPC path tracking 컨트롤러의 성능을 평가하기 위해 **(Fig 13)** 처럼 다양한 속도로 움직이는 obstacle을 고려한다. 
마찰계수 μ = 0.5으로 설정했다. **(Fig 17)** 부터 **(Fig 19)** 까지는 20m/s로 주행할 때 두 가지 접근법(컨트롤러A,B) 에 대한 시뮬레이션의 차이를 보여준다. 
두 개의 컨트롤러는 차량이 계획된 trajector를 따르도록 강제로 steering wheel을 왼쪽으로 꺾으면서 obstacle을 피하려한다. 
컨트롤러A는 불안정해지고, 반면 컨트롤러B는 장애물을 피하고도 여전히 계획된 trajectory를 따라 주행한다.   

![fig17](https://user-images.githubusercontent.com/69246778/126060074-a5c078ce-0d65-4a27-983a-cbba999cfaf9.png)   

**(Fig 17)** 은 차량의 trajectory와 tracking error를 보여준다. 빨간선은 컨트롤러A의 결과, 파란선은 컨트롤러B의 결과이고 검정선은 path planning 프로그램이 
계산한 계획된 trajectory이다.   

컨트롤러A는 충돌 전까지는 차량을 계획된 경로를 따르게 해준다. 그러나 차량이 일단 reference에서 멀리 벗어나게 되면 컨트롤러는 더이상 시스템을 계획된 
trajectory로 끌어올 수 없고 그 시스템은 제어할 수 없게 된다. 이러 유형의 동작은 [tire Saturation]에 의해 유도되고 이건 **(Fig 19)** 처럼 안정성의
threshold를 초과하는 side slip angle과 yaw rate를 유발한다.   

![fig19](https://user-images.githubusercontent.com/69246778/126060584-ba2fd035-e6e3-48f3-8070-4b392c1e6abc.png)   

우리는 prediction horizon Np=20, control horizon Nc=5를 확장함으로써 컨트롤러A의 성능을 향상시킬 수 있다.
그러나, 계산시간이 많이 들고 설명된 실험 platform에서 런타임 오류를 발생시킨다.   

t=4s 이후의 조작(obstacle 회피 이후 가속하여 우측 차선으로 돌아오고 직선 주행)은 **(Fig 18)** 에 나타난 것과 같다. 컨트롤러A는 planned trajectory를 따라
가기 위해 차량을 조종하지도 못하고, 최대 steering angle과 steering rate로도 안정성을 유지하지도 못한다. 컨트롤러B는 항상 차량을 선형 영역에 구속하고 컨트롤
러A 보다 차량의 tracking 성능을 더 좋게 해준다.   

![fig18](https://user-images.githubusercontent.com/69246778/126060761-a45bb84a-09a2-4dbc-8d10-e53ac79518e5.png)   

```
📝NOTE   
컨트롤러A는 충돌 회피 이후에 다시 제자리로 돌아와 안정적인 주행을 하기가 어렵지만   
컨트롤러 B는 안정적인 주행이 가능하고 tracking 성능도 더 좋다.
```

# 7. Conclusion
* * *

##### [Input command]
##### [Tire saturation]
##### [yaw angle](https://www.researchgate.net/figure/Bicycle-vehicle-model_fig2_346390366)
##### [yaw_rate](https://blog.naver.com/jjz0426/221135413776)

