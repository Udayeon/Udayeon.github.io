---
layout: post
title: 
description: |
  강화학습의 원리와 과정
hide_image: true
tags:
  - deeplearning
published: true
use_math: true
---

# 강화학습(Reinforcement Learning)
* * *

# 1. 강화학습이란?
기계(컴퓨터)를 마치 사람을 학습시키듯 훈련시키는 것을 **기계학습**이라 하는데 기계학습의 종류는 다음과 같다.   

|종류|특징|   
|-----------------------------------|--------------------------------------|
|지도학습(Supervised Learning)|학습데이터+정답 모두 알려준다. (분류, 회귀, 예측)|
|비지도학습(Unsupervised Learning)|학습데이터에 대한 정답을 알려주지 않고 기계 스스로 그 구조를 찾도록 한다. (클러스터링, 차원축소)|
|준지도학습(Semi-supervised Learning)|라벨링 된 데이터 + 되지않은 데이터 섞어서 학습|
|**강화학습(Reinforcement Learning)**|**agent가 최고의 보상을 얻는 방향으로 시나리오를 시도하는 것. 어떤 행동을 해야할지 배우는 것이 아님.**|
   
다른 학습과 달리 강화학습은 어떤 행동을 해야할지 **정답을 배우는 것이 아니다.**    
예를 들어, 아기가 첫 걸음마를 떼는 상황을 상상해보자. 이 때 행동을 하는 아기가 **agent**이고 아이에게 피드백을 제공하는 부모는 **환경**이 된다. 
기계학습 과정에 비유하면 다음과 같다.   
   
**agent가 걸었다(agent는 첫 발떼기를 성공한 state)    
→ env의 긍정적 피드백(뭐 표정이 좋다던가, 웃어준다던가)    
→ agent는 또 걷는다 (두 번째 발을 뗀 state)→ env의 긍정적 피드백    
→ 어쩌다 agent가 발을 헛디뎠다(넘어진 state)    
→ env의 부정적 피드백    
→ agent는 발을 헛디디기보다 잘 걷는 방향으로 action을 취한다.**      
   
즉, agent가 행동을 할수록 state가 바뀌고 이에 대해 env의 피드백도 달라짐. 긍정적 보상 또는 부정적 보상을 받게됨. 
결론적으로 **누적보상이 최대가 되는 action을 취하는 agent를 학습하는 것**
![image](https://user-images.githubusercontent.com/69246778/149459768-8b924e67-6ea9-4f1e-b80c-e4a5375f509a.png)

   
# 2. 강화학습 기본 개념들
* **Agent** : 주어진 문제 상황에서 행동하는 주체
* **State** : agent의 상태, 현재 시점에서 가능한 모든 상태의 집합을 **State Space,S**라 하고, 특정 시점 t에서의 상태를 **s_t**라 한다.
* **Reward** : agent가 어떤 행동을 했을 때 따라오는 이득. 높을수록 좋다.
* **Environment** : 문제 세팅 그 자체. agent가 할 수 있는 행동, 그에 따른 보상 등등 모든 규칙. 즉, Agent, State,Reward모두 환경의 구성요소이다.
* **Cost** : Reward에 -1을 곱한 값으로 낮을수록 좋다. 
* **정책** : agent의 행동패턴. 주어진 State에서 어떤 Action을 취할지 말해준다. state를 action에 연결짓는 함수라고 보면 된다.
누적 보상이 최대가 되는 **최적정책**을 찾아야 한다. 
* **MDP** : Markov Decision Process,마르코프결정프로세스. 아래 그림으로 설명되는, 위에서 설명한 관계를 의미한다.
![image](https://user-images.githubusercontent.com/69246778/149460651-bef8c696-0f1d-42a9-95ed-153a4ebfa443.png)

# 3. 예제로 살펴보는 강화학습 원리
사용할 예제는 Matlab에서 제공하는 **[PPO예제](https://kr.mathworks.com/help/deeplearning/ug/train-ppo-agent-for-automatic-parking-valet.html)**   
이 예제는 자동 주차 알고리즘으로 **Adaptive MPC Controller**와 **강화학습**을 사용한다.
**MPC controller**로 빈 주차 공간을 검색해 reference path를따라 일정한 속도로 이동하고 빈 자리에 도착하면 
**강화학습 agent**가 사전 훈련된 주차 maneuver를 실행한다. (빈 공간 및 주차된 차량의 위치를 포함한 환경에 대한 사전 지식은 제공된다.)
   
## 3.1. PPO란?
[Proximal Policy Optimization Algorithms,John Schulman(2017)](https://arxiv.org/pdf/1707.06347.pdf)   
정책 최적화 알고리즘 기법 중 하나이다. PPO등장 이전엔
**TRPO**[Trust Region Policy Optimization,John Schulman(2015)](https://arxiv.org/pdf/1502.05477.pdf)를 사용했다.   
   
Policy는 **Deterministic(결정적)**와 **Stochastic(확률적)** 으로 나눌 수 있는데, 전자는 주어진 state당 하나의 action을 
취하는 것이고 후자는 **주어진 state에 대해 action들의 확률분포를 고려하는 것이다.** **TRPO는 Stochastic Policy**기반의 
최적화기법이다.   
   
**TR(Trust Region)** 은 performance가 상승하는 방향으로 업데이트를 보장할 수 있는 구간을 의미하며 TRPO는 이를 이용해 
최적정책을 찾는 최적화기법이다. 이 performance를 평가하기 위해 
**Performance function**을 이용한다. 그러나, **TRPO는 연산이 복잡하고 호환성이 떨어지는 문제**가 있다. 
따라서, TRPO만큼의 성능은 지키면서 위의 문제를 해결하기 위한 **PPO**가 제안된다.
   
<참고>
([TRPO](https://ropiens.tistory.com/82))
([PPO](https://ropiens.tistory.com/85))
   
## 3.2. Parking Lot
주차장은 **ParkingLot** class로 표현할 수 있다. 각 주차 구역마다 녹색 또는 빨간 불빛으로 자리가 차있는지 비었는지 알 수 있다.
주차된 차량은 검정색으로 표현된다. 다음의 예제에서는 7번 자리가 비어있는 주차장을 사용한다. **Ego vehicle**의 초기 위치를
좌표로 설정한다. 그리고 ego vehicle이 주차 할 **target pose**는 앞에서 설정한 **freeSpotIdx**를 이용해 설정한다. 
```
freeSpotIdx = 7; %7번자리 비었음
map = ParkingLot(freeSpotIdx); %7번자리가 비워진 ParkingLot class

egoInitialPose = [20, 15, 0]; %현재 차량 위치

egoTargetPose = createTargetPose(map,freeSpotIdx) %주차할 위치 = 비어있던 자리
```

## 3.3. Sensor Modules
본 예제는 **Camera**와 **Lidar**를 사용한다. 

* **Camera**   
camera로 비어있는 주차공간을 탐지하고 비어있는 자리와 현재 차량 위치 간의 기하학적 관계를 이용해 동작을 수행한다. 
![image](https://user-images.githubusercontent.com/69246778/149479306-ea037ca1-8a0b-4de2-ae55-f6e27e1bb631.png)
- FOV(φ) = ±60 degrees, d_max = 10m   
![image](https://user-images.githubusercontent.com/69246778/149479454-0112c064-eb6f-4126-9979-651c2de26d05.png)
위와 같다면 d_i와 φ_i는 주차공간까지의 거리 및 각도가 될 수 있다.

* **Lidar**
강화학습 agent는 라이다 센서 결과를 읽어 ego vehicle과 다른 차량 간의 근접성을 평가한다. 라이다 센서도 카메라와 마찬가지로
기하학적 관계를 사용한다. 라이더 거리는 ego vehicle의 중심에서 방사형으로 나오는 12개의 선이 장애물에 닿는 것을 통해 측정된다.
측정 가능한 최대 거리는 6m이다.
![image](https://user-images.githubusercontent.com/69246778/149481283-82ff4332-aad0-4212-a169-4fbcc323d343.png)

## 3.4. Auto Parking Valet Model
model의 시뮬링크를 확인해보자
```
autoParkingValetParams  %parameter실행하고

mdl = 'rlAutoParkingValet';
open_system(mdl) %Simulink열어
```
![image](https://user-images.githubusercontent.com/69246778/149487182-df7cf201-6d50-454e-932f-82d6c56ab34b.png)

* **Ego Vehicle Model**
![image](https://user-images.githubusercontent.com/69246778/149487464-9a03fc51-6cfd-4911-87ad-c713b8615e82.png)
Input으로 **Speed(m/s),Steering angle(rad)** 을 받는다.   
   
**Vehicle Dynamics** subsystem을 보면
![image](https://user-images.githubusercontent.com/69246778/149487773-24213b89-effa-4ddb-9ce7-61daefa91f5d.png)
**Bicycle Model**임을 알 수 있다. 

* **MPC Tracking Controller**
![image](https://user-images.githubusercontent.com/69246778/149487976-38ad09b0-3ec6-4639-92bd-e12d5d5d14f0.png)
**state**에 따라 **Control**이 결정된다. 

* **RL Controller**
![image](https://user-images.githubusercontent.com/69246778/149488138-b5775809-00f0-4345-8873-35059ddc7b83.png)
Vehicle의 현재 위치와 Lidar정보, Target pose(주차할 자리)를 input으로 받아 주차를 수행한다. 단 이를 활성화 할지 말지는
Camera가 결정하게 된다. 

* **Vehicle Model**
![image](https://user-images.githubusercontent.com/69246778/149488664-f74d0d49-ab84-425a-962b-5f4b862cfc34.png)
RL Controller 활성화에 연결되어 있는 **Vehicle model** subsystem을 열어보면  Pose를 input으로 받아 Camera가 
goal을 찾도록 설계되어 있다. 

정리하면, **Camera**알고리즘에 의해 **MPC controller**가 reference path를 tracking하고 빈자리가 발견되면 주차모드가 
활성화 되어 **RL controller**가 주차 maneuver를 수행한다.

## 3.5. Adaptive Model Predictive Controller
```
createMPCForParking %MPC Controller개체 생성
```

## 3.6. Reinforcement Learning Environment
![image](https://user-images.githubusercontent.com/69246778/149490349-3e5bf11b-756b-43b6-989f-d0c9bd1aa059.png)
이렇게 빨간 영역에서 **Training**을 할거임. 주차장이 대칭적이여서 전체에서 하지 않고 이 영역에서 훈련해서 다른 곳에 적응하면 됨.
   
Environment는 다음과 같다
* Training region은 목표지점이 중앙에 위치한 22.5m x 20m 의 사각형이다.
![image](https://user-images.githubusercontent.com/69246778/149491174-6203e0f1-8b2b-4730-a131-03f7d7efa86c.png)
* Observation은 ego vehicle과 target pose의 **error**, 실제 **Heading angle의 sin,cosine**, **Lidar측정값**
* 주차하는 동안 차량의 속도는 2m/s
* action signal은 15도 단계에서 -45도 ~ +45도 사이의 이산적 steering angle이다.
* 차량과 차량 구역과의 오류가 **0.75m이내, 각도는 10도 이내**일 경우에만 주차된 것으로 간주한다.
* ego vehicle이 training region 밖으로 나가거나, 장애물과 충돌하거나, 주차에 성공하면 에피소드가 종료된다.
* 시간 t에서의 보상 r_t는 다음과 같다.
![image](https://user-images.githubusercontent.com/69246778/149719366-d70cef55-4b36-4038-b4fb-039c5b92359d.png)
- X_e : X position error
- Y_e : Y position error
- θ_e : Heading angle error
- δ : Steering angle
- f_t : 시각t에서 차량이 주차를 성공하면 1, 실패하면 0
- g_t : 시각t에서 차량이 장애물과 충돌하면 1, 충돌하지 않으면 0
    
정리하면, **Error가 클수록 보상이 작아진다. 차량이 주차를 성공하면 보상이 커진다. 장애물과 충돌하면 보상이 작아진다.**   

주차 공간변 좌표 변환은 다음과 같다.
![image](https://user-images.githubusercontent.com/69246778/149720136-20fc8df2-e273-43a2-8228-f32e4b626d0e.png)
   
Environment에 대한 **Observation**과 **Action specification**은 다음과 같다.
```
numObservations = 16;
observationInfo = rlNumericSpec([numObservations 1]);
observationInfo.Name = 'observations';

steerMax = pi/4;
discreteSteerAngles = -steerMax : deg2rad(15) : steerMax;
actionInfo = rlFiniteSetSpec(num2cell(discreteSteerAngles));
actionInfo.Name = 'actions';
numActions = numel(actionInfo.Elements);
```
   
Environment 인터페이스를 위한 simulink를 설정한다.
```
blk = [mdl '/RL Controller/RL Agent'];
env = rlSimulinkEnv(mdl,blk,observationInfo,actionInfo);
```
  
Training을 위한 **Reset function**을 설정한다. **autoParkingValetResetFcn**은 각 에피소드 시작 시에 ego vehicle의 초기값을 임의로
재설정한다.
```
env.ResetFcn = @autoParkingValetResetFcn;
```
   
# 3.7. Create Agent
이 예제에서는 **PPO agent**를 사용한 **Actor network**와 **ritic network**를 사용한다.
- **Critic Network** : **policy**의 parameter를 update시킴. 학습대상의 성능을 평가.
- **Actork Network** : **Action-value function**의 parameter를 update.
![image](https://user-images.githubusercontent.com/69246778/149872047-0fde6073-0143-4d9b-acab-b5d116a8c3cc.png)
   
random seed를 생성
```
rng(0)
```
   
**critic representation**,16개의 input과 1개의 output을 가진 심층신경망 만들기. critic network의 결과는 특정 observation에 대한
**state function**이다.
```
criticNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(128,'Name','fc3')
    reluLayer('Name','relu3')
    fullyConnectedLayer(1,'Name','fc4')];
```
   
critic network option 설정
```
criticOptions = rlRepresentationOptions('LearnRate',1e-3,'GradientThreshold',1);
critic = rlValueRepresentation(criticNetwork,observationInfo,...
    'Observation',{'observations'},criticOptions);
```
   
**Actor representation**, actor network의 output은 차량이 특정 state에 있을 때 **가능한 조향 동작을 취할 확률**이다.
```
actorNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(numActions, 'Name', 'out')
    softmaxLayer('Name','actionProb')];
```
   
**Stochastic actor representation** option
```
actorOptions = rlRepresentationOptions('LearnRate',2e-4,'GradientThreshold',1);
actor = rlStochasticActorRepresentation(actorNetwork,observationInfo,actionInfo,...
    'Observation',{'observations'},actorOptions);
```
   
**agent option**설정
```
agentOpts = rlPPOAgentOptions(...
    'SampleTime',Ts,...
    'ExperienceHorizon',200,... %경험에서 배우기 전에 agent가 환경과 상호작용하는 step의 수, 미니배치보다 작아야함
    'ClipFactor',0.2,... %각 policy 업데이트과정에서 변경사항을 제한해줌(default)
    'EntropyLossWeight',0.01,... %(default)
    'MiniBatchSize',64,...
    'NumEpoch',3,...
    'AdvantageEstimateMethod',"gae",...
    'GAEFactor',0.95,... %advantage estimator에 대한 smoothing factor
    'DiscountFactor',0.998); %훈련 중 미래 보상에 적용되는 discount factor
agent = rlPPOAgent(actor,critic,agentOpts);
```

# 3.8. Train Agent
```
trainOpts = rlTrainingOptions(...
    'MaxEpisodes',10000,...      %최대 episode 10000개
    'MaxStepsPerEpisode',200,... %episode당 최대 time step 200
    'ScoreAveragingWindowLength',200,... 
    'Plots','training-progress',...
    'StopTrainingCriteria','AverageReward',...
    'StopTrainingValue',80);
```

```
doTraining = false;    % 여길 true로 바꾸면 직접 교육을 시작함. false이면 사전 훈련된 agent가 load됨
if doTraining
    trainingStats = train(agent,env,trainOpts);
else
    load('rlAutoParkingValetAgent.mat','agent');
end
```
![image](https://user-images.githubusercontent.com/69246778/149876674-ab1a5194-e6f6-4f06-9e75-e3f4201add44.png)
직접 훈련하는 모습을 볼 수 있음
![image](https://user-images.githubusercontent.com/69246778/149876885-2ef5bfe3-a14d-4094-a66b-cd388e59db18.png)

# 3.9. Simulate Agent
```
freeSpotIdx = 7;  % free spot location
sim(mdl);
```

# 4. Results
# 
