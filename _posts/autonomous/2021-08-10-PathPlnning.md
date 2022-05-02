---
layout: post
title: 
description: |
  Path Planning, Path Generation, Path Following, Algorithm
hide_image: true
tags:
  - autonomous
published: true
---

# Path Planning
* * *

# 1. Path Planning
한 지점에서 다른 한 지점까지 가는 동안 주변을 인식하고, 정보를 처리해 적절한 판단을 하여 가야할 경로를 생성하고, 정확한 
제어를 통해 길을 따라가는 일련의 과정. 경로 생성에 필요한 기술은 다음과 같다.                  
- Path generation (경로생성)
- Path following (경로추종) 
- Decision making 
   
## 1.1. Global path planning
**전역경로계획**   
![global](https://user-images.githubusercontent.com/69246778/125191437-68161880-e27d-11eb-8304-0ca8f018b7bb.jpg)   
* **전역 지도** 안에서 출발점-도착점까지 갈 수 있는 **수많은 경로 중 하나**를 선택하는 것.   
* 예를 들어, 강남에서 국민대학교 까지 여러 경로를 기준(최단거리 or 최저비용...)에 따라 결정.   
* 이미 흔히 제공 되는 서비스로 자율 주행 발달에 따라 큰 변화가 필요한 영역은 아님. 

## 1.2. Local path planning
**지역경로계획**   
![local](https://user-images.githubusercontent.com/69246778/125191438-6b110900-e27d-11eb-954c-3f8c35d79159.jpg)   
* 내 차 근처 100~500m 까지의 지역 지도 안에서 **주변 환경을 실시간으로 처리**하여 주행하도록 하는 것.   
* 경로 후보를 **실타래**처럼 뿌려서 그 중 최적의 경로를 선택하고 선택된 경로를 따라감.   
* **자율 주행 발달시 실시간으로 계속 선택해야 하므로 중요한 영역**   
![obsX](https://user-images.githubusercontent.com/69246778/125191295-99daaf80-e27c-11eb-8472-000365237479.jpg)   
![obsO](https://user-images.githubusercontent.com/69246778/125191312-b0810680-e27c-11eb-846e-4fd323b0c215.jpg)   
두 지점 사이의 위험 요소들을 피하는 경로를 선택하되 너무 돌아가면 효율성이 떨어지고, 너무 가로지르면 안전성이 떨어짐.   
* **효율성과 안전성을 잘 조절해야 함.**   
* **실시간 처리가 필요하므로 너무 무거운 알고리즘을 쓰면 안됨**   

## 1.3. Challenges
|                               |                                                |
|:------------------------------|:-----------------------------------------------|
|**기본적인 차선 유지 및 차선 변경** |                                                |
|**좌/우회전**                      |차선이 없을 수도 있음, 주변 차량도 모두 살펴야함|
|**고속도로 진입 및 이탈**          |                                                |
|**오토바이, 자전거, 킥보드**       |살짝만 비껴가는 주행(human-like)                |
|**고속도로의 화물차가 느린 경우**  | 차선 변경 후 다시 돌아오기|
|**임시 공사 구간**                |                                                |
|**통신 기반 정보에 따른 주행**     |각 차량 간의 주행 경로가 겹치지 않게             |
|**열악한 날씨**                   |눈,비로 인해 마찰이 낮아지는 경우 제동거리 변화 고려 (적응형 주행전략)|

**안전성과 효율성의 적정 지점 찾기**

## 1.4. Perception in path planning
perception한 정보의 종류에 따라 경로 계획이 달라짐.

|정보                      |경로 계획|
|:------------------------ |:-------|
|**object 유/무와 주변의 위치**| Search region에 Object가 있나 없나만 따져서, 있다면 걍 멈춰버림(답답..)|
|**+ 속도**                   |[Collision avoidance](https://udayeon.github.io/2021/07/11/path-planning/#collision-avoidance) 계산해서 주행전략 수립|
|**+ 종류**                   |Human-like 한 주행 (약간만 비껴간다거나..)|

# 2. Path generation
어떤 경로를 끊임없이 실시간으로 생성해서 선택하는 알고리즘

## 2.1. Lane Keeping
경로 생성의 가장 간단한 기술 중 하나   
   
1. Deep Learning, Classical Vision 기술로 차선 인식
2. 차선 중심에 Set Point 도출 (경로 생성기술)
3. 그 Set Point 들을 다항식 보간법을 이용해 차로의 중심 Line 형성
4. 그 Path를 추종
{:.message}
   
## 2.2. Challenges
|                                          |                                                        |
|:-----------------------------------------|:-------------------------------------------------------|
|**주변에 장애물X, 모든 차가 자기 자리를 지킴** |차선 인식해서 걍 따르면 됨                               |
|**주변 차량 인접 or 화물차가 옆에**            |주변 차량 고려한 미세조정 필요                           |
|**갓길 주차차량 or 오토바이**                  |Human-like한 주행으로 살짝만 비끼기                      |    
|**장애물 없는 교차로**                        |신호만 고려                                             |
|**복잡한 교차로**                              |주변 동적개체 고려해 우선 순위 판단하여 다양하게 주행가능|

## 2.3. A* Algorithm
* 출발점에서 목적지까지 최단거리를 구하는 그래프/[트리](https://udayeon.github.io/2021/07/11/path-planning/#tree-structure)
알고리즘 중 하나.

### 2.3.1. features
1. 현실 세계를 2D grid로 표현하고 Grid Map 상에서 최단 경로를 계속해서 탐색해서 판단.
![A1f1](https://user-images.githubusercontent.com/69246778/125193588-19ba4700-e288-11eb-8784-88d01797c9d6.jpg)   
2. Robotics 분야 - 로봇의 경로 생성.   
3. 자율주행 분야 - 로봇보다 제한적이므로 활용 가능.      
4. 8 방향의 격자에 대한 Cost를 계산해 최적의 경로를 찾는다.   
![A1cost](https://user-images.githubusercontent.com/69246778/125193623-51c18a00-e288-11eb-9180-b365d3e5f71e.jpg)     
5. 격자의 크기에 따라 특성이 다름.   
![A1res](https://user-images.githubusercontent.com/69246778/125193666-8d5c5400-e288-11eb-8dcf-4bc708afee1c.jpg)   
- 크게 나누는 경우 : 격자 수가 적으므로 계산량에 부담은 없어 실시간으로 돌리기에 유리함
- 작게 나누는 경우 : 현실 세계를 더 잘 표현할 수 있지만 격자가 많아 계산량이 늘어난다.   
**따라서 적정한 수준의 격자를 찾는게 중요하다. obstacle 크기에 기반해 표현이 가능한 적절한 단위 선택**   

### 2.3.2. Process
![1](https://user-images.githubusercontent.com/69246778/125193762-2be8b500-e289-11eb-8201-88dce9308cc9.jpg)
![2](https://user-images.githubusercontent.com/69246778/125193879-bdf0bd80-e289-11eb-846c-fb71c0fd71bc.jpg)   
- **경로 선택 실패** : Open List가 비어 있는 경우, 조사할 부분이 없음을 의미하므로 길이 없다고 판단.   
- **경로 선택 성공** : 목표한 사각형이 Open List에 들어가는 경우, 앞으로 조사할 경로 중에 목적지가 포함되는 것.    
**🚨주의🚨**   
* 트리 구조 알고리즘은 경로 선택 과정이 시작에서 끝으로 가는 것이 아니라, 끝에서 시작으로 거꾸로 가는 것.   
* 목표한 사각형이 Open list에 들어가는 경우에, 그 때부터 도착점부터 출발점까지 재귀적으로 (부모 노드를 가리키는 방향으로)   
Path를 잇는다.

## 2.4. RRT Algorithm
* **Rapidly-exploring Random Tree**
* 무작위 샘플링을 사용하여 고차원의 구성공간을 탐색하는 경로 계획 알고리즘.
![RRT](https://user-images.githubusercontent.com/69246778/125198047-3c099000-e29b-11eb-9a1c-4f34f73b8020.jpg)
* Xrand와 가장 가까운 점 Xnear사이에 임의의 점 Xnew를 설정하고 Xrand를 이 새로운 Xnew로 편입시킴.   
 그리고 다시, Xrand를 뿌린 후 Xrand와 가장 가까운 점 Xnear를 구하고 Xrand와 Xnear 사이에 임의의 점 Xnew를 설정.   
 Xrand를 이 새로운 Xnew에 편입. 위 과정을 목적지에 다다를 때 까지 반복한다.
* 중간에 장애물에 걸리면 새로운 Xrand를 뿌린다.
   
### 2.4.1. Features
* **샘플링 기반**에 의거한 방식
* 시작점과 목적지가 정해지면 임의의 Xrand를 뽑아 검색트리 T를 계속 확장. (신경망,나뭇가지처럼...)   
* 이때, Xrand는 일반적으로 균등분포를 사용해 선택하고 목적지에 빠르게 다다르기 위해 목적지에 치우친 분포를 사용할 수도 있음.   
* **🚨주의🚨**  트리 T가 목적지에 다다르면 목적지부터 시작점까지 **재귀적으로** Tree를 검색하여 경로를 탐색.   

### 2.4.2. Pseudo Code
![RRTseudo](https://user-images.githubusercontent.com/69246778/125198056-42980780-e29b-11eb-8e18-01094a79204d.jpg)
{:.message}

## 2.4.3. limit
랜덤으로 샘플링하므로 최적화(Optimality)를 보장하진 않음. 따라서 RRT* 등장.

## 2.5. RRT* Algorithm
![rrtstar](https://user-images.githubusercontent.com/69246778/125198183-d4a01000-e29b-11eb-8c36-7a6ff32e24b3.jpg)
* RRT알고리즘에서 **Optimality를 향상** 시킨 알고리즘으로 최적의 이동 계획을 위한 Cost function을 도입.   
* 새로운 State를 뽑을 때마다 new state의 Neighbor들을 Optimal path로 [rewiring](https://udayeon.github.io/2021/07/11/path-planning/#rewiring).   
* Xnew의 일정 반경 내의 어떤 node들의 후보 set를 추리고 그 후보 set에 최적 검사를 해서 Best parent Node를 설정.   
* 즉, 새롭게 뿌린 Xrand와 가장 가까운 점을 단순히 연결하는 것이 아니라 Xrand를 둘러싼 여러 후보 set에서 최적인 점을 찾아 다시 연결할 수도 있음. 그러면 새로운 부모 노드가 탄생하게 됨.

### 2.5.1. Pseudo Code
![RRTstarseudo](https://user-images.githubusercontent.com/69246778/125198194-df5aa500-e29b-11eb-813b-6035f12c6540.jpg)
{:.message}

## 2.6. RRT vs RRT*

|     공통점   |
|:----------------------|
| - **실시간성** 보장 되어야 함 (obstacle정보도 고려)|
| - **적정량의 트리구조** (너무 많으면 실시간성이 떨어지고 적으면 정확도가 떨어진다.)|
{:.message}

# 3. Path following
* 경로 생성 과정을 통해 선택된 실시간 경로를 차량이 정확하게 따라가게 하는 기술로 적절한 **조향각과 속도**를 결정.   
* 차량이 움직이는 것을 조절하므로 **제어**와 밀접한 관련.   
* **실시간성을 보장**해야 한다. 
* Lane keeping으로 결정한 차선을 cm단위로 정확하고 정밀하게 추종하는 것이 중요. 크기가 큰 차량의 경우 방향이 조금만 틀어져도 옆 차선을 침범할 위험이 있기 때문이다.   

## 3.1. Challenges
|           |                                                                                |
|:----------|:-------------------------------------------------------------------------------|
|**고속일 경우**| 같은 각도 변화라도 저속에 비해 lateral position의 발생이 크므로 정밀 주행이 필요.|
|**좁은 골목길**| 낮은 속도로 섬세하고 정밀한 조향각 제어 필요|
|**극심한 각도의 코너**| Human-like한 주행 필요|

## 3.2. Pure Pursuit
![pure1](https://user-images.githubusercontent.com/69246778/125227247-7f044b80-e30d-11eb-99fd-6e7768084ce9.jpg)
* 하나의 점을 선택해 경로를 추종하는 방식으로 목표 경로를 실시간으로 추종하는 기하학 기반의 알고리즘.   
*  **제어**와 밀접한 관련.   
* **차량의 Path의 한 점을, 원호를 그리며 따라가는 방법**이라 할 수 있다.  

## 3.2.1. Features 
* 차량 뒤축 위치, 차량 진행 경로 목표점을 잇는 원호 곡률을 기하학적으로 계산.
* 하나의 목표 지점의 골라서 지속적으로 추종할 수 있는 Steering angle을 발행
* * **목표점** : 차량 뒤축에서 원하는 방향으로 일정거리 떨어진 곳
* **steering angle** : 차량 진행방향 벡터와 목표점 좌표를 향하는 목표 벡터 사이 각도의 차만으로 결정

## 3.2.2. Look-ahead distance 
![purehigh](https://user-images.githubusercontent.com/69246778/125227267-89264a00-e30d-11eb-9c51-577f20e417a8.jpg)
![purelow](https://user-images.githubusercontent.com/69246778/125227259-86c3f000-e30d-11eb-99b4-2fdbe8673560.jpg)
* path의 수많은 점들 중 어떤 점을 잡을지 **기준**을 제시하는 값.    
* **속도와 비례**하므로 저속일 때 가까운 점을 선택하고, 고속일 때 먼 점을 선택.   
{:.message}

## 3.2.3. Look-ahead distance setting
* 가야하는 path의 정보(Way Point)를 미리 알 수 있으면 좋다.   
  EX) 직진하다가 극심한 코너를 미리 인지하여 Look-ahead distance를 미리 줄인다.
* Maximun/Minimum bound가 필요하다. 너무 작거나 크면 성능이 불안정하다.
* Velocity Control: Obstacle이 존재하면 제동거리 기반의 가/감속 결정   
![CodeCogsEqn](https://user-images.githubusercontent.com/69246778/125229028-dbb53580-e310-11eb-89fe-55b97ea0e6ff.gif)   
(d_2:제동정지거리, v,V : 주행속도, g:중력가속도, f:노면-타이어 간 종방향 미끄럼 마찰계수)

## 3.3. Model Predictive Control, MPC
* 목표 경로를 최적으로 추종하여 쫓아가는 알고리즘
* Path를 인위적으로 바꿔가면서 실시간 대응
* 차량 **제어** 와 밀접한 관련
* **현재정보 + 예측정보(찰나의 짧은 시간 후를 예측)** 를 이용해 안정성을 높임 

## 3.3.1. Pure pursuit VS MPC

|**Pure pursuit**          |**MPC**                |
|:-------------------------|:-------------------|
|* 기하학적 접근방법        |* 모델 예측 접근 방법 | 
|* 다양한 동력학적 변수 제외 |* 실시간 예측 및 추종 |
|* 미래 예측 어려움         |* 광범위한 범위에서 주목받는 연구 분야|   
{:.message}

## 3.3.2. [PID] VS MPC

|**PID**                   |**MPC**             |
|:-------------------------|:-------------------|
|* Gain parameter 잘못 설정하면 oscillation 발생|* 예측의 영역이 포함됨|
|* Closed loop|           |                
|Ex) 오른쪽으로 치우치면 왼쪽으로,   왼쪽으로 치우치면 오른쪽으로...|       |
{:.message}



## 4. 기타
##### Tree structure
그래프의 일종으로 여러 노드가 한 노드를 가리킬 수 없는 구조.   
최상위 노드인 Root node와 부모노드(Parent node), 자식노드(Child node)로 구성   

##### Rewiring
best parent 생성 후 주변 node를 재해석, 경로 체크하여 edge를 끊거나 만들어 냄.   


##### Stanley Method
- 스탠포드 대학에서 무인 자동차를 위해 만든 조향 제어 알고리즘
- 앞 차축 중심으로부터 가장 가까운 지점까지 측정되는 경로이탈 오차의 비선형 함수 피드백 사용
![stanley](https://user-images.githubusercontent.com/69246778/125260260-2186f380-e33b-11eb-8931-619986795fc0.jpg)


##### PID(Proportional-Integral-Differential)
- 제어의 근간을 이루는 **feedbck loop**의 기본적 방법
- **비례-적분-미분** 제어 방법의 피드백 제어기 형태 
1. 제어하고자 하는 조향각의 출력값 측정
2. 원하는 참조값 혹은 설정값과 비교하여 오차 도출
3. 이 오차값으로 제어에 필요한 제어값 계산

