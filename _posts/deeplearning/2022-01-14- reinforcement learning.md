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
**agent가 걸었다 -> env의 긍정적 피드백(뭐 표정이 좋다던가, 웃어준다던가) -> agent는 또 걷는다 -> env의 긍정적 피드백 -> 어쩌다 agent가 발을 헛디뎠다 
-> env의 부정적 피드백 -> agent는 발을 헛디디기보다 잘 걷는 방향으로 action을 취하게 될것.**   
즉, **env의 피드백을 기반으로 agent는 보상이 최대가 되는 action을 취한다.**

