---
layout: post
title: Path Planning
description: |
  K-mooc 자율주행자동차기술(국민대학교) 강의 부분
tags:
  - autonomous
author: Udayeon
published: true
---

# Path Planning
- Path Planning
  - Definiton
  - Type
    - Global Path Planning
    - Local Path Planning
  - Challenges
  - Perception in path planning
- Path Generation
- 


# Path Planning
* * * 

## Definition

한 지점에서 다른 한 지점까지 가는 동안 주변을 인식하고, 정보를 처리해 적절한 판단을 하여 가야할 경로를 생성하고, 정확한 제어를 통해 길을 따라가는 일련의 과정.
경로 생성에 필요한 기술은 다음과 같다.                                                                
- Path generation (경로생성) 
- Path following (경로추종) 
- Decision making 

## type

### Global path planning

**전역경로계획** 전역 지도 안에서 출발점-도착점까지 갈 수 있는 수많은 경로 중 하나를 선택하는 것.
예를 들어, 강남에서 국민대학교 까지 여러 경로를 기준(최단거리 or 최저비용...)에 따라 결정한다.
이미 흔히 제공 되는 서비스로 자율 주행 발달에 따라 큰 변화가 필요한 영역은 아님.

### Local path planning

**지역경로계획** 내차 근처 100~500m 까지의 지역 지도 안에서 주변 환경을 실시간으로 처리하여 주행하도록 하는 것.
경로 후보를 실타래처럼 뿌려서 그 중 최적의 경로를 선택하고 선택된 경로를 따라감.
**자율 주행 발달시 실시간으로 계속 선택해야 하므로 중요한 영역임!**

<1지역경로계획그림>

**(1)obstacle 없는 경우**

<2obstacle없는 경우그림>

Lane keeping 기반

**(2)obstacle 있는 경우**

<3obstacle 있는 경우 그림>

두 지점 사이의 위험 요소들을 피하는 경로를 선택하되
너무 돌아가면 효율성이 떨어지고, 너무 가로지르면 안전성이 떨어진다.
**효율성과 안전성을 잘 조절해야 함.**
**실시간 처리가 필요하므로 너무 무거운 알고리즘을 쓰면 안됨**


## Challenges

|                               |                                                |
|:------------------------------|:-----------------------------------------------|
|기본적인 차선 유지 및 차선 변경 |                                                |
|좌/우회전                      |차선이 없을 수도 있음, 주변 차량도 모두 살펴야함|
|고속도로 진입 및 이탈          |                                                |
|오토바이, 자전거, 킥보드       |살짝만 비껴가는 주행(human-like)                |
|고속도로의 화물차가 느린 경우  | 차선 변경 후 다시 돌아오기|
|임시 공사 구간                |                                                |
|통신 기반 정보에 따른 주행     |각 차량 간의 주행 경로가 겹치지 않게             |
|열악한 날씨                   |눈,비로 인해 마찰이 낮아지는 경우 제동거리 변화 고려 (적응형 주행전략)|

**안전성과 효율성의 적정 지점 찾기**

## Perception in path planning

perception한 정보의 종류에 따라 경로 계획이 달라진다.

|정보                      |경로 계획|
|:------------------------ |:-------|
|object 유/무와 주변의 위치| Search region에 Object가 있나 없나만 따져서, 있다면 걍 멈춰버림(답답..)|
| + 속도                   |Collision avoidance 계산해서 주행전략 수립|
| + 종류                   |Human-like 한 주행 (약간만 비껴간다거나..)|



# Path generation
* * *

## Definition

어떤 경로를 끊임없이 실시간으로 생성해서 선택하는 알고리즘

## Lane Keeping
경로 생성의 가장 간단한 기술 중 하나

### Process
1. Deep Learning, Classical Vision 기술로 차선 인식
2. 차선 중심에 Set Point 도출 (경로 생성기술)
3. 그 Set Point 들을 다항식 보간법을 이용해 차로의 중심 Line 형성
4. 그 Path를 추종

### Challenges

|                                          |                                                        |
|:-----------------------------------------|:-------------------------------------------------------|
|주변에 장애물X, 모든 차가 자기 자리를 지킴 |차선 인식해서 걍 따르면 됨                               |
|주변 차량 인접 or 화물차가 옆에            |주변 차량 고려한 미세조정 필요                           |
|갓길 주차차량 or 오토바이                  |Human-like한 주행으로 살짝만 비끼기                      |    
|장애물 없는 교차로                         |신호만 고려                                             |
|복잡한 교차로                              |주변 동적개체 고려해 우선 순위 판단하여 다양하게 주행가능|

## A* 알고리즘


# Path following
* * *
##
