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
  - Introduction
  - Description of collision avoidance system
  - Path Planning for collision avoidance using 3-D virtual dangerous potential field
  - Vehicle mathematical model for path-tracking problem 
  - Design fo multiconstrained model predictive control
  - Simulations of path tracking in different scenarios using carsim and simulink
  - Conclusion

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

MMPC설계법에 modeling이 필요하므로 Path tracking 문제는 차량 modeling에 의존함. 본 논문에서는 차량의 kinematic 및 dynamic한 측면을
고려한 model이 필요함. 여기서 제안되는 것은 collsion avoidance system 개발에 사용되는 augmented mathematical model임. 
[Section 4.A.] 에서는 lateral 및 yaw dynamic을 고려한 vehicle dynamic model을 개발하고, [Section 4.B.]에서는 MMPC 개발에 사용되는 
discrete state-space model을 소개함.

```
📝NOTE
MMPC를 설명하기 위해 차량의 운동학적 및 동적 측면을 고려한 모델인 augmented mathematical mode를 제안함.
이는  collsion avoidance system 개발에 사용되는 model임.
section A : lateral 및 yaw를 고려한 vehicle dynamic model개발
section B : MMPC개발에 사용되는 discrete state-space model 소개
```

## 4.A. Vehicle Dynamic Model for Path Tracking
```
📝NOTE
lateral 및 yaw를 고려한 모델 개발
```

Control design에 사용될 차량과 타이어 modeling을 설명함. 먼저 path tracking 문제에서 vehicle modeling은 다음의 가정을 전제로 함.
- longitudinal velocity는 일정.
- 앞축과 뒤축에서 좌우 바퀴는 single wheel 하나로 묶음.
- suspensions movement, slip 현상, 공기역학의 효과는 없다고 가정.   
   
이러한 가정을 통해 **(Fig 19)** 와 같은 linear dynamic model을 그릴 수 있고 이는 뉴턴의 법칙에 따라 구해진 것임.   
![fig9](https://user-images.githubusercontent.com/69246778/126086178-1ff13692-8dd8-4b54-9d24-7c62f6a8093b.png)   
차체의 sideslip angle β 와 차체의 yaw rate ψ˙를 state variable로 보고, 차량의 lateral dynamics는 다음과 같이 쓸 수 있음.      
![image](https://user-images.githubusercontent.com/69246778/126086210-ffad5d77-0948-426c-b1fe-ac9533ace72f.png)   
   
Iz : yaw 축에 관한 차량의 Inertia   
β  : sideslip angle   
ψ˙ : yaw rate   
lf : CG(Center of Gravity)로 부터 앞 바퀴까지의 거리      
lr : CG(Center of Gravity)로 부터 뒤 바퀴까지의 거리      

cornering tire force에 대해 다양한 model이 많이 존재함. tire slip angle이 작을 때, lateral tire force는 
tire slip angle의 선형함수로 근사할 수 있음. 따라서 다음과 같이 쓸 수 있음.   
![image](https://user-images.githubusercontent.com/69246778/126086712-db4d1cef-0d99-44ef-a4cf-9d9ecaf02e96.png)   
   
F_{xf} : front tire force   
F_{xr} : rear tire force   
α_f : front tire slip angle   
α_r : rear tire slip angle   
δ : front wheel streeing angle   
C_f : cornering stiffness of front    
C_r : cornering stiffness of rear   

식 (14),(15)를 식(12),(13)에 대입해서 구한 다음의 식은 lateral 및 yaw dynamics을 다루는 식을 정의.   
![image](https://user-images.githubusercontent.com/69246778/126086686-cc2a9733-6421-490d-b426-9db65bac174c.png)    
![Page3](https://user-images.githubusercontent.com/69246778/126087981-24205fe5-fa21-40e0-b647-d104ea186056.jpg)   
   
```
📝NOTE
MMPC를 설명하기 위해 차량의 kinematic과 dynamic을 고려한 차량 model이 필요하므로 
collison avoidance system 개발에 사용되는 augmented mathematical model을 제안함.
이 모델의 latera dynamic(식12,13)과 tire force(식14,15)에 관한 식을 연립해
lateral과 yaw dynamic를 다루는 방정식을 표현할 수 있음(식 16,17)
```

## 4.B. Discrete linear vehicle model for MPC
```
📝NOTE
Section A에서 lateral 및 yaw를 다루는 식을 유도했고
이 식으로부터 MMPC최적화를 위한 discrete state-space식을 유도.
```
MMPC최적화를 위해 이전 section에서 얻은 mathematical model로부터 discrete-state-space model을 유도.
새로운 vehicle model에서, state-space vector는 CG의 lateral position, side slip angle, yaw angle, yaw rate로 구성됨.
그리고 input은 front wheel streeing angle임. 따라서 다음과 같이 표기할 수 있음.   
![image](https://user-images.githubusercontent.com/69246778/126088279-dfc09e92-3595-4fbd-9369-2092f0243825.png)   
   
X_c : state-space vector   
X_v : CG의 lateral position   
β  : sideslip angle   
ψ  : yaw angle   
ψ˙ : yaw rate   
   
상태 방정식은 이전 section에서 유도된 **(식16)** 과 **(식17)** 에 기반해 다음과 같이 쓸 수 있음.   
![image](https://user-images.githubusercontent.com/69246778/126088300-81dca464-3706-4f6d-904e-dbbac2357633.png)   
![image](https://user-images.githubusercontent.com/69246778/126088319-7515207b-0ea8-4620-8604-7a8342efc3a3.png)   
   

앞에서 언급한 차량 모델은 선형화 된 *연속시간 및 단일입력, 다중출력 시스템*. 그러나, 제어될 시스템은 일반적으로 [literatue 29]의
discrete state-space model에 의해 modeling. 따라서, **(식 19)** 와 **(식 20)** discrete state-space model로 변환되어 다음과 같은 식을 얻음.   
![image](https://user-images.githubusercontent.com/69246778/126089046-275d70a2-804e-486c-a610-25848954305c.png)   
   
A_d : state matrices   
B_d : control matrices   
이는 다음과 같이 [오일러 method]로 계산될 수 있음.

![image](https://user-images.githubusercontent.com/69246778/126089228-edaea7ed-59cc-4225-8b09-447b314181c7.png)

ΔT : discrete state-space model의 sampling 간격   
lateral displacement(변위), sideslip angle, yaw rate는 다음의 식을 사용해 output으로 정의됨.   
![image](https://user-images.githubusercontent.com/69246778/126089544-36931240-e6ac-4436-a116-e8f05c18251b.png)   
![image](https://user-images.githubusercontent.com/69246778/126089764-bafc6d47-0d61-46be-b8a9-35a98a8d7897.png)   
   
MMPC를 사용한 Path tracking에서 constrained control 문제를 실시간 최적화 문제로 공식화하는 것이 일반적. discrete state-space model인
**(식22)**와 **(식25)** 를 통합해 단일화된 model로 확장시킬 수 있음.   
상태변수와 제어변수의 차이를 다음과 같이 표현   
![image](https://user-images.githubusercontent.com/69246778/126090282-274c43f9-69bc-4853-a62d-62081b0702da.png)   
   
**(식 27)~(식 29)** 를 **(식 22)** 와 **(식 25)** 에 대입하면 다음과 같이 쓸 수 있음. 이는 변수 Xd(k)와 u(k)의 증분으로 표현된 discrete
state-space model.   
![image](https://user-images.githubusercontent.com/69246778/126092679-8dae6c72-c36f-47f0-b903-84748c0167cc.png)   
   
Δu(k) : state-space model과 output 방정식에 대한 input   
ΔX_d(k)를 output인 Y(k)와 연결하기 위해 새로운 state variable vector X_a(k)를 다음과 같이 설정.   
![image](https://user-images.githubusercontent.com/69246778/126092694-e31ad49b-ed3a-4f06-8ec9-4d22a23648df.png)   
   
**(식 32)** 를 **(식 30)** 과 **(식 31)** 에 결합하면 다음과 같은 state-space model이 만들어짐.      
![image](https://user-images.githubusercontent.com/69246778/126092718-15a86e0e-07a5-4205-b4d6-556992f2f15d.png)   
![image](https://user-images.githubusercontent.com/69246778/126092727-ea85f757-af98-41e3-a18e-136f0771e7af.png)   
여기서 (Aa,Ba,Ca)는 augmented model(증강모형)이라 부른다. 

```
📝NOTE
앞에서 얻은  mathematical model로 부터 discrete state-space model을 유도함.
이전 section에서 유도된 식16,17을 통해 상태방정식 식19,식20을 유도할 수 있음.
이때, X_c는 state-space vector로 lateral position, sideslip angle, yaw angle, yaw rate로 구성되어 있고
steering angle(δ)은 input으로 받음.

그런데 discrete state-space modeling된 시스템을 다뤄야하므로 식19,20을 변환하여 식22,25를 유도함.
식 22는 state matrices와 control matrices를 포함하고
식 25는 lateral displacement, sideslip angle, yaw rate를 output으로 정의하게 해줌.
state variables과 control variables의 차이를 표현한 식27,28,29를 식 22,25에 적용하여
식 30,31을 유도하고 이때 새로운 state varible ventor X_a(k)를 설정하면
최종적으로 식33,34같은 state-space model이 만들어짐. 
```

# 5. Design fo multiconstrained model predictive control
* * *
path tracking 은 차량 역학과 운동학에서 발생한 constraint에 대한 예측제어 문제가 될 수 있음. 
여기에 제시된 분석은 차량 collison avoidance application에 적합하도록 조정된다. 
   
## 5.A. Prediction of State and Output Variables
path tracking을 위한 MPC의 디자인에 있어서 각 시간마다 차량의 미래 행동을 예측하는 것이 중요함.
이 미래 예측은 특정한 prediction horizon 내에서 control input을 결정해주고 미래 상태에 기초하여, 최적화된 control input을 계산하기 위하여 
performance index가 최소화 됨. 
주어진 정보 X_a(k)를 이용한 future state variable은 N_p step에서 다음과 같이 예측.   
![image](https://user-images.githubusercontent.com/69246778/126094305-0d296d3c-a82d-421a-8a8c-f599279bb58d.png)   
   
X_a(k) : state variable vector, 현재 plant 정보를 제공하고 측정을 통해 사용할 수 있음.
k : 현재 시각 (k>0)   
N_p : 10, prediction horizon(length of optimization window)   
N_c : 5, control horizon      
X_a(k + m) : 현재의 plant정보 X_a(k)를 통해서 예측한 k + m 에서의 state variable.
   
현재 관측된 상태에 대해 시간 k에서 계산된 future input increment의 시퀀스를 다음과 같이 나타냄.   
![image](https://user-images.githubusercontent.com/69246778/126094555-2dd82bac-de7c-47ee-b357-812348e21114.png)   
ΔU_m : set of future control parameters
   
state variables은 다음의 식처럼 **(식 33)** 의 반복계산을 통해 차례로 계산됨.
![image](https://user-images.githubusercontent.com/69246778/126094782-424e9e13-8c82-4bc8-9aa8-d47992ef6a54.png)   
   
A_a,B_a,C_a : state-space model(식 35에 표현되어 있는 augmented model)   

연속적인 대입에 의해, control input은 오직 N_c(the control horizon) time step에서만 변화하고, 이후(preview,prediction horizon)까지 
일정하게 유지된다고 가정.   
그리고나서 predictive state-space model을 위해 state 벡터와 predicted output을 다음과 같이 정의.   
![image](https://user-images.githubusercontent.com/69246778/126095130-0e4a0914-153b-4141-904e-54a39682fe73.png)   
X_m(k) : state vector for predictive state-space model   
Y_m(k) : predicted output   
   
이런 상황에서 N_p에 대한 성능을 출력하는 예측모델을 다음과 같이 유도할 수 있음.   
![image](https://user-images.githubusercontent.com/69246778/126095699-b2b711fe-eb01-42a8-9e8a-42e1073b9026.png)   
![image](https://user-images.githubusercontent.com/69246778/126095743-7bd6d036-7aba-410a-ae37-0d2e1e8e176d.png)   

```
📝NOTE
시각 k에서의 주어진 plant 정보를 바탕으로 k+m에서의 상태 X_a(k+m)를 예측할 수 있음.
future input의 증분은 ΔU_m으로 나타내게 됨.

X_a(k+m)과 ΔU_m을 이용해 식33을 반복계산하면 state variables를 차례로 계산할 수 있음.
predictive state-space model을 위한 state vector 와 output을 정의할 수 있고
그에 따라, 이 prediction model의 성능을 평가하는 식도 간단하게 표현할 수 있음
```

## 5.B. Developent of Cost Function With Vehicle Dynamics
[section 3]에서 설명한 것 처럼, N_p 이내의 도로와 obstacle의 universal potential field에 의해 계산된 MMPC의 set-point 정보로써,
시각 k에서 계획된 trajectory P_r(k), sideslip angle β_r(k), yaw rate ψ˙_r(k)의 reference location정보가 선택됨.   
reference signal은 다음과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126099850-749145ab-f5d4-4db8-8b36-715e14671629.png)   
    
P_r(k) : 시각 k에서 계획된 trajectory (referece가 되는 거)   
β_r(k) : 시각 k에서 계획된 sideslip angle   
ψ˙r(k) : 시각 k에서 계획된 yaw rate   
   
MMPC의 목적은, 예측되는 outputs(P_v(k),β_r(k),ψ˙r(k))을 set-point에 최대한 가깝게 만드는 것. 비용 함수 J_E를 control objective를 
반영해 다음과 같이 정의. 
![image](https://user-images.githubusercontent.com/69246778/126100432-21ab8b4a-3ed0-4bfb-8b1a-210a6c5a4db1.png)   

P_v(k) : 지구좌표계, 시각 k에서 N_p의 time step으로 예측되는 lateral position   
β_r(k) : 지구좌표계, 시각 k에서 N_p의 time step으로 예측되는 sideslip angle   
ψ˙r(k) : 지구좌표계, 시각 k에서 N_p의 time step으로 예측되는 yaw rate   
ΔU_m : predicted optimization vector   
![image](https://user-images.githubusercontent.com/69246778/126100886-09c545ab-c045-40ab-94a7-407e31e25072.png):steer input의
future value와 관련된 cost function 매트릭스.  

reference sideslip angle은 다음과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126114592-c4f6e4f3-3134-4495-b655-906e10303d76.png)   
   
V : 자율주행 차량의 일정한 종방향 속도   
C_T : 시각 t에서의 계획된 경로의 곡률   
L=L_f + L_r  : 차량 축거리
X_T : 고정 지구 좌표계에서 계획된 trajectory의 x좌표(universal potential field에서 유도)
Y_T : 고정 지구 좌표계에서 계획된 trajectory의 x좌표(universal potential field에서 유도)

참고문헌 [32]와 [33]에 따르면 β_curv를 [-β_max, β_max]로 제한해야 함. 이때, β_max는 다음과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126114770-4752db33-652b-49f9-900d-05740527e610.png)   
   
V_r : 전체 속도   
k_1 : pi/18, [32]에서 참조한 reasonable한 parameter   
k_2 : pi/60, [32]에서 참조한 reasonable한 parameter   

sideslip angle 의 reference signal은 다음과 같음   
![image](https://user-images.githubusercontent.com/69246778/126115647-607ee473-0df9-4f49-820e-5a88b6663b58.png)   
   
β(k)가 [-β_max, β_max]를 벗어나면 β_r(k)의 크기는 β_max로 규제됨.   
   
자율주행 차량의, 원하는 yaw rate 는 다음과 같이 정의.   
![image](https://user-images.githubusercontent.com/69246778/126118030-dbe37bf4-817e-4fcc-a320-4ce6f3b8e586.png)   

Ψ'_ curv : 원하는 yaw rate (reference)   
V : 속도   
C_T : 곡률   
β_curv : 그때의 sideslip angle   
   
![image](https://user-images.githubusercontent.com/69246778/126118402-fac3d57e-2ab5-4a8f-8112-f9d89327ed7d.png)   
   
μ : 마찰계수   
g : 중력가속도
V : 종방향 속도
   
yaw rate의 reference signal은 다음과 같음.
![image](https://user-images.githubusercontent.com/69246778/126118453-e8ae77db-d6c6-43a7-bfca-1327b5ca02c4.png)   
   
앞서 언급한 경우에서, yaw rate와 sideslip angle이 범위를 벗어날 때 yaw rate와 sidelslip angle의 추적오류에 대한 패널티가 
급격히 증가할 것이며 이는 비용함수에서 중요한 역할.   
  
**(식 44)** 는 다음과 같이 쓸 수 있다.   
![image](https://user-images.githubusercontent.com/69246778/126119384-391fc07f-e721-4a34-87e3-d85e49e9718d.png)   
![image](https://user-images.githubusercontent.com/69246778/126119283-075491cd-2a12-4e21-8da3-981d1807607e.png)    
   
R_r(k) : universal potential field로부터 구한 reference signal   
Y_m(k) : path tracking을 위한 predicted output   

```
NOTE📝

```


## 5.C. Constraint Analysis for MPC
path tracking에서 발생하는 constraint에는 크게 세 가지 유형이 있음. 첫 번째 두 개는 control 변수에 부과되는 constraint를 다루고,
세 번째는 output constraint를 다룸.   
차량 모델의 운동학과 동역학에 따르면, steering angle과 안정성, path tracking 문제에 부과되는 constraints는 다음과 같음.
![image](https://user-images.githubusercontent.com/69246778/126122722-fd247caa-7ffa-4555-8953-73642732a1be.png)   

δ_max, Δδ_max : input constraint   
X_max, X_min은 X방향 도로의 좌우 경계 좌표 값   
   
![image](https://user-images.githubusercontent.com/69246778/126123150-e6c2ec12-3449-4a7a-87d2-cfbb5877a1a6.png)   

불균등한 constraints를 받는다.
![image](https://user-images.githubusercontent.com/69246778/126435341-c9e8a23e-416a-4d9c-b37e-8a759b5942ce.png)   
![image](https://user-images.githubusercontent.com/69246778/126435378-3edbd5f3-2335-4582-8bb6-4de688b9d6b3.png)   

## 5.D. Numerous solution using Hildreth's Quadratic programming procedure   
다음 단계는 차량의 path tracking 응답을 최적화하는 future steering input을 계산하는 것. **(식 41),(식 53),(식 58)** 을 이용한
목적함수 J_E와 constraints를 나타냄.
![image](https://user-images.githubusercontent.com/69246778/126435855-506fb28d-9b5f-4ebd-aee7-15c9c1f274ec.png)   
![image](https://user-images.githubusercontent.com/69246778/126435885-aa39be6a-f25d-4b14-bdd7-7f2e224f9149.png)   
![image](https://user-images.githubusercontent.com/69246778/126435910-da82f2f3-316e-4b23-ba14-7a4624a5bdbb.png)   
   
E_m, F_m : quadratic 문제에 호환되는 matrice와 vector   
   
불평등 제약을 받는 목적함수를 최소화하기 위해 다음과 같은 라그랑주 식을 고려.
![image](https://user-images.githubusercontent.com/69246778/126436165-8a2209cb-f443-45e8-b90d-65315c81bda4.png)

original primal 문제에서의 이중적인 문제는, 비용함수 J_L의 1차 도함수로부터 발생하는데, 그 최소화는
ΔU_m에 대해 unconstrain되어 있고 다음의 식에서 얻을 수 있다.   
![image](https://user-images.githubusercontent.com/69246778/126437149-7f9b9496-8821-4ad4-86d9-817ee9f4b42b.png)   
이 식을 **(식 65)** 에 대입하면   
![image](https://user-images.githubusercontent.com/69246778/126437210-c302ad11-1844-4d00-87d8-354c5cff646d.png)   
   
이중적인 문제는 decision variable λ를 포함한 quadratic programming 문제로 바뀜.
![image](https://user-images.githubusercontent.com/69246778/126437352-92d06cc4-cec0-4d2a-989e-030135c37bc3.png)
![image](https://user-images.githubusercontent.com/69246778/126437366-1a750762-2fae-4af1-8320-7df7efc99695.png)   
   
Hildreth's quadratic 프로그래밍 절차는 이중 목적함수를 최소화하는 optimal한 라그랑주 multiplier를 해결하기 위해 선택됨.
process의 주어진 단계에서 벡터 λ* 의 단일 구성 요소 λ_i를 조정하여 목적함수를 최소화. λ* 의 계산 절차는 **(Fig 10)**.
**(식 66)** 의 decision variable λ를 라그랑주 multiplier λ* 로 대체한 결과는 아래 식.
![image](https://user-images.githubusercontent.com/69246778/126438149-0de0420c-f3c3-4ec0-904b-2b197f714d77.png)


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

![table1](https://user-images.githubusercontent.com/69246778/126023542-fdddd7c4-255422222222222222222222222222222-47a1-917f-a9d63ab34946.png)   

**Scenario 1**   
선두 차량이 10m/s의 속도로 달릴 때, 20m/s의 일정한 속도로 달리고 있는 host 차량은 **(Fig 12)** 의 빨간색으로 나타낸 궤적을 따라가게 제어되고 
차량이 obstacle과 충돌하지 않는지 계속해서 확인한다.   

![fig12](https://user-images.githubusercontent.com/69246778/126061123-10e8f42c-65e7-41ee-ba39-09f3177a6933.png)   

```
📝NOTE   
왜1 빨간색???빨간색은 V=0일 때 아닌가
```

**(Fig 14)** 부터 **(Fig 16)** 은 설계된 MPC기반 경로 추적 컨트롤러A와 컨트롤러B의 성능을 비교하고, trajectory 응답, 차량 응답 등을 각각 나타낸다.   
- time 0s ~ 2.5s : obstacle을 통과하기 전, 두 컨트롤러 좌회전함으로써 obstacle을 피하기 위한 경로를 따라가는 것을 볼 수 있다.   
- time 2.5s ~ 6s : 장애물을 통과하고 나면 두 개의 컨트롤러는 차량을 오른쪽 차선의 중심선으로 다시 가지고 온다.   
  
![fig14](https://user-images.githubusercontent.com/69246778/126059526-eb4d88a1-b9f1-4a57-bb8b-302533c9bd79.png)   

**(Fi111g 14)** 에서, 컨트롤러B는 컨트롤러 A보다 더 나은 path-tracking 성능을 나타낸다. 

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

