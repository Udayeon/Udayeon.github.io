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

# (Review) Path Planning for Autonomous Vehicles using Model Predictive Control

[Path Planning for Autonomous Vehicles using Model Predictive Control](https://ieeexplore.ieee.org/document/7995716/metrics#metrics)   
- **Author** Chang Liu1, Seungho Lee2, Scott Varnhagen2, H. Eric Tseng2   
- 2017 IEEE Intelligent Vehicles Symposium (IV), June 11-14, 2017, Redondo Beach, CA, USA
{:.message}

**논문 선정 기준**   
* 자율주행 기술 동향이 잘 반영된 것(너무 오래되지 않은 것)
* 인용 횟수 많은 것
* 국내/국외 구분은 하지 않음
* 분량이 적당한 것   
선택한 논문은 자율주행 기술 중 경로추종, 그 중에서도 최근 각광 받고 있는 예측모델을 이용한 경로추종 방법에 대한 내용을 포함한다.   
2017년 발표 이후 약 4600회의 피인용 횟수를 기록했고 분량은 6페이지로 2주 안에 조사하고 공부하기에 적당하고 생각했다.   
   
  - [Abstract](https://udayeon.github.io/2021/07/14/Path-Planning-for-Autonomous-Vehicles-using-Model-Predictive-Control/#abstract)
  - [Introduction](https://udayeon.github.io/2021/07/14/Path-Planning-for-Autonomous-Vehicles-using-Model-Predictive-Control/#introduction)
  - [Problem formulation](https://udayeon.github.io/2021/07/14/Path-Planning-for-Autonomous-Vehicles-using-Model-Predictive-Control/#problem-formulation)
  - [Path planning algorithm](https://udayeon.github.io/2021/07/14/Path-Planning-for-Autonomous-Vehicles-using-Model-Predictive-Control/#3-path-planning-algorithm)
  - [Simulation results](https://udayeon.github.io/2021/07/14/Path-Planning-for-Autonomous-Vehicles-using-Model-Predictive-Control/#4-simulation-results)
   
# Abstract
* * *
동적환경에서의 경로 계획은 중요하지만, 차량 동력학적 제약과 주변의 차량을 고려해야 하므로 어려움.
일반적으로 차량의 trajectory는 차선유지, 차선변경, 진입로 합병, 교차로 횡단 등에 따른 다양한 조작 mode를 수반하는데 
이러한 mode 전환을 위해 rule-based high-level decision making 기반의 선행기술이 존재함. 
**그러나 특정한 규칙을 규정하기보다 mode전환을 자동으로 수행하는, MPC 기반의 통일된 경로계획을 제안**   
안전을 보장하기 위해 **주변 차량을 다각형으로 모델링**하고 ego vehicle과 주변 차량 간의 **충돌 회피를 수행하기 위한 MPC constraints**를 개발한다.
안전하고 자연스러운 조작을 위해 MPC의 목적함수에 **차선과 관련한 potential field**를 포함한다.
제안된 방법을 다양한 test 시나리오에서 시뮬레이션했고, 그 결과는 안전을 보장하면서 자동으로 합리적인 조작을 제공하는 이 접근법이 효율적임을 증명한다. 

# Introduction
* * *
지난 10년간 자율주행 차량이 크게 발전함에 따라 도로안전의 향상과 생산적인 이동시간 보장을 꾀하게 되었음.
자율주행을 강화시키기 위해 중요한 기술 중 하나인 경로 계획은 **차량의 움직임이 nonlinear, non-holonomic**하고, **주변 차량 및 인프라와의 
충돌을 피하면서 원하는 성능을 유지**해야 한다는 점에서 어려운 의사결정 및 제어 문제임. 또한 원하는 속도로 주행하거나 운전자를 보호하는 등의 조작도 필요해짐.   
   
연구는 경로 계획의 효율을 증가시키기 위해 계속 발전해왔음. 초기에는 주로 **그래프 검색 기반 플래너**를 사용했는데, 
이는 환경을 이산적으로 구성한 그래프에서 **최단 경로를 생성**하는 방식임.
잘 알려진 방법으로는 **Dijkstras(다익스트라), A* 알고리즘** 이 있음, 이런 격자 state 알고리즘은 **실시간 경로 계획**을 가능케 함.
이 방법들이 **효율성과 최적화**를 보장함에도 불구하고 **계획된 trajectroy의 품질은 그래프의 해상도에 따라 크게 달라질 수 있고** 
게다가, **경로계획 과정 중에 차량 dynamics 요소를 고려하기가 쉽지 않음.**
**곡선보간계획법(Clothoid, Polynominal, Spline, Bezier 등등)** 도 널리 사용되었음.
이 방법들은 그래프 검색기반(다익스트라, A* 등)과 마찬가지로 **소수의 제어점 또는 매개변수로만 곡선의 행동을 정의**할 수 있기에 
**계산비용이 적다**는 장점이 있음. 그러나 **생성된 궤적의 최적화가 보장되진 않는단** 게 단점.
또한, 경로 계획 과정에서 **차량의 동력학적 요소가 고려되지 않으므로** 발생한 경로를 다듬어서 그것들을 차량에 동적으로 실현할 수 있는 과정이 필요함.   
   
RPM과 RRT같은 샘플링 기반 방법이 최근 인기임. 그것들은 **holonomic, non-holonomic 두 경우 모두에 있어서 실현가능한 trajectory를 생성**한다는 
장점이 있음. RRT* 는 심지어, 발생한 trajectory의 **점근적인 최적화를 보장**하고 **샘플의 수를 늘리면 전역 최적 솔루션에 거의 수렴**하기도 함.
이러한 샘플링 기반의 방법들과 그것의 변형들은 연구 커뮤니티에서 자율주행에 널리 적용되고 있음. 그러나, 이런 접근법의 실질적인 사용은 
**샘플링 절차의 계산적인 복잡성**에 의해 방해받고 있어서 **실질적 사용이 어려움.**   
   
**trajectory optimization**에 기반한 접근법은 최근 자율주행 경로 계획에 있어 점점 주목받고 있음. 핵심적인 아이디어는 **최적화 문제로써 
경로계획을 계산**하는 것임. 최적화 문제는 **원하는 차량의 성능, 그리고 관련 constraints를 고려**하는 문제임.
이런 접근법의 장점은 **원하는 경로의 모든 요구 조건을 쉽게 인코딩할 수 있는 유연성과 용이성** 
그리고, 최근의 CVX와 Ipopt 같은 최적화 해결사의 발전은 **실시간으로 빠르고 신뢰할 수 있는 경로를 생성**할 수 있게 함.
전통적으로, **trajectory optimization**기반의 접근법은 시작점에서 목표점까지 이어지는 완전한 trajectory를 해결하지만, 
주행 환경은 보통 **동적이고 확률적이므로 완전히 예측하기 어려움.**
그래서 최근 자율주행의 경로 계획에 있어서 **MPC방법**이 흔히 사용됨.
**MPC는 제한된 시간의 궤도 최적화 문제를 재귀적인 방식으로 해결하고 경로 계획 과정 중에 환경 상태를 업데이트하여 
고려할 수 있음. 그러므로, 우리는 자율주행의 온라인 경로계획에 MPC방법을 사용**   
   
일반적으로 차량 trajectory는 도로 위에서 다양한 종류의 조작을 포함함. 이전의 작업들은 이런 다양한 조작을 주행모드와 
분리해서 다뤘고 mode들 중의 하나만을 제어하는데 집중했음. 자율주행차량의 모드 전환을 위해 high-level decision making 방법이 제안되기도 햤음.
그러나 이런 방법은 보통 **설계하기 힘들고 예기치 않는 상황에서의 신뢰도가 떨어짐**   
   
이 연구에서, **통일된 framework하에서 다양한 주행 모드를 다룰 수 있는 MPC기반의 경로계획을 제안**
특정한 조작을 위해 **미리 정해진 규칙에 따르는 것 대신, 비선형적인 MPC문제를 온라인으로 해결함.**
차선변경, 차선유지, 진입로 합병, 교차로 횡단 등과 같은 **다양한 조작은 모두 MPC로부터 생성한 경로에 의해 자동으로 결정**
**차선변경와 차선유지**를 위해 MPC의 목적함수 내에 **rRelaxed convex combination method**를 활용함.
주행안전을 보장하기 위해 **주변 차량을 다각형으로 모델링**하고 자차와 주변 차량 간의 **충돌 회피를 수행하기 위해 MPC constraints를 개발**
또한, **차선과 관련한 potential field**도 개발함. 이건 **같은 차선에 있는 주변 차량에 적용되어 안전하고 자연스러운 조작을 가능케함.**   
제안된 방법은 시뮬레이션에서 평가되었고 그 결과는 안전을 보장하면서 자동으로 합리적인 조작을 제공하는 이 접근법이 효율적임을 증명함.   

# Problem formulation
이 연구에서 고려된 자율주행 시나리오는 **(Fig 1)** 과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126581933-e6ca077f-9cc0-4f02-a87a-e141b41b6f70.png)   
고속도로와 도시 주행 상황을 모두 포함해 구조화 된 환경에서 자율주행차량(ego vehicle)은 도로에 의해 규정되는 속도로 지정된 목적지까지 주행.
차량이 주변 차량과 충돌하지 않고 편안한 주행환경을 유지해야 함.   
      
동작 제어의 2가지 단계는 다음과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126582196-334ab38b-19ba-47a5-9b1e-8f2b4b78b156.png)   
상위 단계의 컨트롤러는 **Path planner**로 환경과 운전자에 관련한 정보를 받아들여 자율주행차량을 위한 **Reference Trajectory 생성**,
하위 단계의 컨트롤러는 **Path Tracker**로 위에서 구한 trajectory를 정확하게 추종하도록 **차량에 직접적인 control input을 적용** 
목표한 효율의 달성을 위해 **Path planner**는 **단순화된 kinematic model을 사용**하고 **Path tracker**는 **high-fidelity (충실도가 높은)
 dynamic model을 활용**함. 이 논문은 Path planner에 초점을 맞춤.   
      
각각의 time step에서 차량은 **MPC를 사용해 finite-horizon을 위한 경로를 계획함.**
**최적의 control sequence를 계산**한 다음 **해당하는 control input을 시행**함. 
그리고 나서 **업데이트된 차량 상태에서 시작해 위의 순서를 각 time step마다 반복.**
이 절차는 차량이 목적지에 도달할 때 까지 **receding horizon** 방식으로 이루어짐.
planning horizon에서 **도로 형태**는 오프라인에 맵핑된 **waypoint와 차선표시에 근거한 4차 다항식**에 의해 **온라인에 근사**됨.   

# 3. Path planning algorithm
* * *

## 3.A. Vehicle and Environment Modeling
본 연구에서 다음과 같은 **도로 좌표계(Fig 3)**을 사용함.   
![image](https://user-images.githubusercontent.com/69246778/126582596-556f2e6e-4a07-416b-94a6-9b090a9ebe06.png)   
![road_coord](https://user-images.githubusercontent.com/69246778/126582910-35c7bd99-7129-4c36-8f6c-937ca774a34e.gif)   
   
차량과 주변 차량을 **unicycle kinematic model**을 사용해 modeling한 식은 다음과 같음.   
![image](https://user-images.githubusercontent.com/69246778/126292806-ebe2bb54-272f-4468-a73e-1f71b3903e83.png)      
![image](https://user-images.githubusercontent.com/69246778/126583084-940f7e46-0715-40fe-839c-7c28b013ef35.png)   
![2](https://user-images.githubusercontent.com/69246778/126586373-68cee803-5c8f-4df5-8963-32b8591fdfb9.gif)   
   
도로의 Waypoint, lane marking 그리고 주변 차량의 현재상태를 포함한 환경정보를 모두 알고 있다고 가정.
또한 주변 차량의 future state도 정확하게 예측된다고 가정.   

## 3.B. MPC Formulation 
MPC를 이용한 접근법은 **receding horizon방식**으로 **유한시간의 constrained optimal control을 해결.**
path planning은 다음과 같은 **비선형 최적화 문제**로 공식화 될 수 있다.
이 목적함수는 차량의 거동을 설명하기 위해 다양한 term으로 구성되어 있음
![image](https://user-images.githubusercontent.com/69246778/126586535-63edf054-fabf-4364-acb5-299a6b0484b9.png)   
![3](https://user-images.githubusercontent.com/69246778/126587181-a63f798b-6f68-4af1-914d-ec362ffdf336.gif)   
![4](https://user-images.githubusercontent.com/69246778/126588453-99782389-cd32-443a-b9ef-d0cd0cb2ca99.gif)

### 3.B.1 Lane Selection
h(x_k,y_k,p_c^j)^2 : k시간에 (xk,yk)에 존재하는 차량과 j번째 차로의 중심선 사이의 lateral 거리.
이 때, pcj는 4차 다항식으로 정의할 수 있다. 차량이 머물 차선을 MPC를 이용해 결정하기 위해 차량이 차선 선택을 반영한 변수가 포함된 혼합정수문제를 정의함. 
그러나, 이 문제를 non-convex programming으로 풀기에는 (1b)와 (1d)의 계산이 매우 버거우므로 실제 구현에는 적합하지 않음. 그러므로 이 문제를 convex relaxtion로 전환하고 
그걸 비선형의 프로그래밍 문제로 바꿨음.   
![image](https://user-images.githubusercontent.com/69246778/126295652-8a572ca2-483b-4998-bc05-6c5eef65e3ad.png)   
![image](https://user-images.githubusercontent.com/69246778/126295723-da99a5ae-c310-47f3-9fd5-9616ff25869f.png)

M * N 행렬로 λ를 정의하고, 설계 변수는 λ_j,k(j,k항목)임.
λ의 진짜 목적은 prediction horizon에서 차량이 어떤 차선에 있어야하는지 결정하기 위한 것.
λ 행렬의 
각 열은 모든 M개의 차선의 convex 조합에 대해 multiplier역할을 해줌.
각 행은 서로 다른 time step에 대한 multiplier임.
λ는 Λ_M * N 에 속하고 이건 R_MN으로 정의된 단위 simplex 집합이다.
   식   
   
### 3.B.2 Lane-Associated Potential Field
안정적이고 자연스러운 조작을 위해 lane-associated potential field를 소개함. 이 식은 차량과 같은 차선에서 달리는 차량에 적용됨. potential field의 식은 다음과 같음.
![image](https://user-images.githubusercontent.com/69246778/126446444-0c3703c6-9a27-4634-865f-c1f43a01fef4.png)
![image](https://user-images.githubusercontent.com/69246778/126446526-f920448e-6d04-40e4-aa9c-bd84127692df.png)

V : index set of surrounding vehicles   
d_j,k : 시각 k일 때, j번째 차선에 함꼐 있는 주변 차량과 자차 간의 종방향 거리를 나타냄.    
s : potential field의 maximum magnitude   
Γ : potential field 의 slope.   
   
   
Γ는 j번째 고속도로의 차량 속도 v와 비례하게 설정.(속도 빠를수록 potential field magnitude기 빠르게 증가) 그럼 더 빠른 차량의 potential
까지 범위가 확대 가능.
![image](https://user-images.githubusercontent.com/69246778/126297647-4eccdb3a-3b9e-4f9b-9e49-443cfb19fd52.png)   
   

### 3.B.3 Collison Avoidance
potential field가 차량과 다른 것들 간의 특정 거리를 유지하도록 돕지만 추가적으로 충돌회피를 보장하는 매카니즘이 필요. 
종/횡 방향 모두에서 차량의 안전을 보장하기 위해 MPC에서 일종의 constraints를 가지고 충돌 회피를 시행함.   
자차와 주변 차량을 흔히 사용하는 원이나 타원 대신 차량의 모양을 더 잘 나타내는 사각형으로 모델링함. 게다가, 주행 환경에서 만나는 
obstacle은 주로 바리게이트나 공사장 이므로 다면체로 나타내기 더 좋음. 그러므로 이건 바람직한 MPC를 활용 다면체 충돌 회피 전략 개발임.
충돌회피는 두 다면체가 교차하지 않아야하며 이를 다음과 같이 쓸 수 있음.   

![image](https://user-images.githubusercontent.com/69246778/126411913-e767ef56-33a8-4206-87a2-e55d558b3cef.png)   
![image](https://user-images.githubusercontent.com/69246778/126411964-c2c4adbe-b762-47df-b092-88d085beb413.png)   
   
A, b : 적절한 차원   
   
그러나 이 조건은 변수가 없어야 하므로 MPC 공식에는 적절하지 않음. 대신, MPC는 특정 condition을 만족하는 변수를 찾는데
적절함. 이 문제를 해결하기 위해 [25]에서 제안한 접근방식을 따름. 이 접근 방식은, 충돌회피 조건을 MPC 공식과 호환되는 
dual form형태로 변화시킴.   
구체적으로, Farka의 부명제(?)에 따르면 **(식 3)** 의 등가조건은 다음과 같음   
![image](https://user-images.githubusercontent.com/69246778/126448321-d044eddf-9652-4f39-9ea7-e7d1fd357297.png)   
   
그러므로, MPC의 constraint로써 **(식 3)** 을 사용하기 보다 새로운 변수β와 **(식 4)** 을 포함한 충돌 회피 constraint
식을 제안. 이게 위에서 설명한 비선형 최적화 문제 식의 **(식 1d)** 임.   
다음과 같은 상태로 표현되는 사각형의 두 차량을 고려함.   
![image](https://user-images.githubusercontent.com/69246778/126449014-74bf292d-b1b7-4fda-8759-3947362787f8.png)   
   
i : =1,2 해당하는 차량과 관련된 index   
W_i : 차량의 width   
H_i : 차량의 length   
![image](https://user-images.githubusercontent.com/69246778/126449512-d7c7ea69-683e-4670-a5c3-2ae53b1ff673.png)   
   
회피해야 하는 주변 차량 q를 가정함. **(식 1d)** 는 다음과 같이 표현됨.   
![image](https://user-images.githubusercontent.com/69246778/126449748-7329d8b4-7748-46ce-a04d-748140de4cc5.png)   
   
ABar : 
bBar :
m

## 3.C. Fail-safe Strategy
차량 kinematic model과 충돌회피 constraint때문에 MPC문제는 비선형의 nonconvex문제임. 따라서 solver가 실현가능한 
solution의 계산에 실패할 수도 있음. 실현 가능한 초기 해결책(initial solution)은 이 문제를 제거하는데 도움이 될 것.
문제 해결을 위해 두 가지 완화된 MPC문제를 제안함. 첫 번째 완화 MPC문제는 목표 함수가 상수로 설정된다는 것 외엔 기존
MPC와 동일함. 이 MPC문제가 실현 가능한 solution을 성공적으로 계산하면 그 solution은 기존 MPC가 seed solution으로 
사용. 그러나, 만약 첫 MPC가 실패하면 두번째 문제로 넘어감. 두번째 문제에서는 충돌회피 constraint를 추가로 제거하게 됨.
시뮬레이션을 통해 이 두가지 완화된 MPC 문제가 항상 실현가능한 solution을 계산해 실현 불가능한 문제를 제거하고 성능을 향상
시킨다는 것을 발견.

# 4. Simulation results
MPC path planner는 normal highway driving, ramp merging, intersection crossing을 포함한다. MPC formulation   
**(식 1a) ~ (식 1d)** 는 Matlab의 YALMIP toolbox를 통해 Ipopt 비선형 프로그래밍 solver로 해결됨.

## 4.A. Normal Highway Driving
**(Fig 5)** 는 noraml한 고속도로 주행환경의 계획된 경로를 캡처한 것임.   
![image](https://user-images.githubusercontent.com/69246778/126575211-477f4fb1-3c86-4237-8eb7-cee9979bcd40.png)   
파란색 상자 안의 차선은 4차 다항식으로 근사됨. 빨간색 직사각형은 주변차량, 파란색 사각형은 ego vehicle. ego 차량의 future
trajetory는 일련의 파란 점으로 표현.   
**(Fig 5(a))** : ego 차량은 중간 차선에 있고 앞쪽에는 다섯 대의 차량이 있고 왼쪽에 한 대가 있음   
**(Fig 5(b))** : ego 차량의 원하는 속도가 앞에 있는 obstacle의 속도보다 빠르지만 ego vehicle이 속도를 감소 시키면서 
일정한 간격을 유지하고 있는걸 알 수 있음.   
**(Fig 5(c))** : 차선 변경이 가능해지자마자 ego vehicle은 속도를 증가시켜 왼쪽 차선으로 부드럽게 바꿈.   
**(Fig 5(d))** : 그리고 왼쪽 차선을 계속 유지함.   
도로의 곡률은 일정하고 view point가 그에 따라 회전. 

## 4.B. Merging in Highway Driving
**(Fig 6)** 은 ramp merging시나리오를 보여줌. 
![image](https://user-images.githubusercontent.com/69246778/126576141-e7710672-d622-456f-a176-5e0bea1adb89.png)   
**(Fig 6(a))** : 주변 차량 3대가 병합 차선에 있음.
**(Fig 6(b))** : ego차량은 안전한  longitudinal(세로) 방향의 안전한 경로를 생성하여 인접한 두 차량 
사이의 간격을 맞춤. ego vehicle은 gap을 따라잡기 위해 먼저 가속함.
**(Fig 6(c))** : 그리고나서 앞차와의 편안한 거리 유지를 위해 감속.
**(Fig 6(d))** : 그러면 ego vehicle이 성공적으로 차선에 병합됨.

## 4.C. Intersection Crossing
**(Fig 7)** 은 4방향 교차로를 건너는 시나리오.   
![image](https://user-images.githubusercontent.com/69246778/126576393-805a9ec6-282a-42ce-85c6-ce60e64746d2.png)   
**(Fig 7(a))** : **(식 1a)** D(y_k)항 설정을 통해 target은 'stop'으로 선택된다.   
**(Fig 7(b)~7(d)** : ego 차량은 먼저 stop 표시에 접근해 거기서 멈추고 교차로를 건너는 것이 안전해질 
때까지 'stop'상태를 유지한다. 

# 5.Conclusion
본 연구에서는, 구조화된 주행 환경에서 자율주행 차량을 위한 MPC기반의 경로 계획을 제안함. 제안된 planner는
통일된 최적화 프레인워크에서 기동 mode를 자동으로 결정함. Convex relaxtion 접근법은 차선을 변경하고
유지하는데 사용됨. 우리는 다각형 형태의 차량에 대한 충돌 회피 조건을 개발했음. 그리고 안정적이고 
자연스러운 주행을 위한 차선관련 potential field도 소개함. 시뮬레이션은 차선 변경, 차선 유지, ramp 병합,
그리고 교차로 횡단의 경우에서 수행됨. 결과는 제안된 path planner가 자율주행 차량을 위한 안전하고 편안한 경로 만들어내는 것을 보여줌. 미래의 연구는 실제 경험에서의 path plnner의 수행에 초점을 맞출 것. 
주변 차량의 움직임에 대한 예측 또한 다음 단계의 과제임. 

##### [Heading angle:Bicycle Model](https://archit-rstg.medium.com/two-to-four-bicycle-model-for-car-898063e87074)
##### [prediction horizon](https://www.igi-global.com/dictionary/prediction-horizon/23230)
예측 모델이 얼마나 멀리까지 미래를 예측하는지를 나타냄. Predictino Horizon이 입력과 출력 사이의 지연과 잘 맞으면 보다 빠르게 제어할 수 있고
좋은 성능을 달성할 수 있다.
##### [Simplex](https://en.wikipedia.org/wiki/Simplex)
n차원 벡터 공간에 n+1개의 점을 가지는 볼록 집합을 말함
##### [Convex set](https://terms.naver.com/entry.naver?docId=3474742&cid=58439&categoryId=58439)
[convex,non-convex](https://www.oreilly.com/radar/the-hard-thing-about-deep-learning/?twitter=@bigdata)
[convex combination](https://blog.naver.com/wjddudwo209/222400921429,https://en.wikipedia.org/wiki/Convex_combination)
[convex combination](https://blog.naver.com/atelierjpro/221174354585)
convex set : 기하학적 집합에 포함되는 임의의 2개의 점을 결합하는 직선이 다시 그 기하학적 집합에 포함되는 경우의 집합.


##### [Polynomianl](https://www.researchgate.net/publication/291827239_Lane_Information_Fusion_Scheme_using_Multiple_Lane_Sensors)
차선의 모델은 사용 목적  및  센서 특징  및  탐지범위에  따라 1차, 2차 또는 3차 다항식 기반의 차선 모델로 모델링  할  수  있다(그림 2). 1차 다항식 모델은 차량  주행  정보  중  차선 오프셋과 방향성 정보만 추출되어  사용이 적으며 곡률 정보가 가미된 2차원 이상의 모델이 주류를 이룬다.  본  연구에서는 곡률의 변화가 선형적으로  이루어진다는 가정을 바탕으로 하는 차량의 3차  다항식으로 표현한 모델[1]을  기준으로 차선 정보를 추출한다.3차 다항식 모델은 식 (1)을 기반으로 하며 각 계수의  의미는 (lateral offset), (heading angle), (curvature), (curvature derivative)이다.fxCCxCxCx=+ + +
3차 다항식으로 차선을 표현하는 건 찾았지만 4차 다항식으로 표현하는건 어케하는건지?

