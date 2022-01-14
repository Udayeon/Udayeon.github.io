---
layout: post
title: 
description: |
  강화학습의 원리와 과정
hide_image: true
tags:
  - deeplearning
published: true
---

# 강화학습(Reinforcement Learning)
* * *

# 1. 강화학습이란?
기계(컴퓨터)를 마치 사람을 학습시키듯 훈련시키는 것을 **기계학습**이라 한다. 기계학습의 종류는 다음과 같다.   
   
- 지도학습(Supervised Learning) : 학습데이터+정답 모두 알려준다. (분류, 회귀, 예측)
- 비지도학습(Unsupervised Learning) : 학습데이터에 대한 정답을 알려주지 않고 기계 스스로 그 구조를 찾도록 한다. (클러스터링, 차원축소)
(- 준지도학습(Semi-supervised Learning) : 레이블 된 데이터 + 되지않은 데이터 섞어서 학습)
- **강화학습(Reinforcement Learning) : agent가 최고의 **보상**을 얻는 방향으로 시나리오를 시도하는 것. 어떤 행동을 해야할지 배우는 것이 아님.**
   
다른 학습과 달리 강화학습은 어떤 행동을 해야할지 정답을 배우는 것이 아니다.    
예를 들어, 아기가 첫 걸음마를 떼는 상황을 상상해보자. 이 때 행동을 하는 아기가 **agent**이고 아이에게 피드백을 제공하는 부모는 **환경**이 된다. 
기계학습 과정에 비유하면 다음과 같다.   
**agent가 걸었다(agent는 첫 발떼기를 성공한 state) → env의 긍정적 피드백(뭐 표정이 좋다던가, 웃어준다던가) → 
agent는 또 걷는다 (두 번째 발을 뗀 state)→ env의 긍정적 피드백 → 어쩌다 agent가 발을 헛디뎠다(넘어진 state) 
→ env의 부정적 피드백 → agent는 발을 헛디디기보다 잘 걷는 방향으로 action을 취한다.**   
즉, **agent가 행동을 할수록 state가 바뀌고 이에 대해 env의 피드백도 달라짐. 긍정적 보상 또는 부정적 보상을 받게됨. 결론적으로 누적보상이 최대가 
되는 action을 취하는 agent를 학습하는 것**
![image](https://user-images.githubusercontent.com/69246778/149459768-8b924e67-6ea9-4f1e-b80c-e4a5375f509a.png)

   
# 2. 강화학습 기본 개념들
* **Agent** : 주어진 문제 상황에서 행동하는 주체
* **State** : agent의 상태, 현재 시점에서 가능한 모든 상태의 집합을 **State Space,S**라 하고, 특정 시점 t에서의 상태를 **s_t**라 한다.
* **Reward** : agent가 어떤 행동을 했을 때 따라오는 이득. 높을수록 좋다.
* **Environment** : 문제 세팅 그 자체. agent가 할 수 있는 행동, 그에 따른 보상 등등 모든 규칙. 즉, Agent, State,Reward모두 환경의 구성요소이다.
* **Cost** : Reward에 -1을 곱한 값으로 이땐 낮을수록 좋다. 
* **정책** : agent의 판단 방식. 누적 보상이 최대가 되는 최적정책을 찾아야 한다.
* **MDP** : Markov Decision Process,마르코프결정프로세스. 아래 그림으로 설명되는, 위에서 설명한 관계를 의미한다.
![image](https://user-images.githubusercontent.com/69246778/149460651-bef8c696-0f1d-42a9-95ed-153a4ebfa443.png)

# 3. 예제로 살펴보는 강화학습 원리
사용할 예제는 Matlab에서 제공하는 **[PPO예제](https://kr.mathworks.com/help/deeplearning/ug/train-ppo-agent-for-automatic-parking-valet.html)**
