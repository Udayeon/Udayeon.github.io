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
* * * 

## Definition
한 지점에서 다른 한 지점까지 가는 동안 주변을 인식하고, 정보를 처리해 적절한 판단을 하여 가야할 경로를 생성하고, 정확한 제어를 통해 길을 따라가는 일련의 과정.
경로 생성에 필요한 기술은 다음과 같다.                                                                
- Path generation (경로생성) 
- Path following (경로추종) 
- Decision making 

## type

### Global path planning
** 전역경로계획** 전역 지도 안에서 출발점-도착점까지 갈 수 있는 수많은 경로 중 하나를 선택하는 것.
예를 들어, 강남에서 국민대학교 까지 여러 경로를 기준(최단거리 or 최저비용...)에 따라 결정한다.
이미 흔히 제공 되는 서비스로 자율 주행 발달에 따라 큰 변화가 필요한 영역은 아님.

### Local path planning
** 지역경로계획 ** 내차 근처 100~500m 까지의 지역 지도 안에서 주변 환경을 실시간으로 처리하여 주행하도록 하는 것.
경로 후보를 실타래처럼 뿌려서 그 중 최적의 경로를 선택하고 선택된 경로를 따라감.
** 자율 주행 발달시 실시간으로 계속 선택해야 하므로 중요한 영역임! **

## 당면 과제

|기본적인 차선 유지 및 차선 변경 |                                                |
|좌/우회전                      |차선이 없을 수도 있음, 주변 차량도 모두 살펴야함|
|고속도로 진입 및 이탈          |                                                |
|오토바이, 자전거, 킥보드       |살짝만 비껴가는 주행(human-like)                |
|고속도로의 화물차가 느린 경우  | 차선 변경 후 다시 돌아오기|
|임시 공사 구간                |                                                |
|통신 기반 정보에 따른 주행     |각 차량 간의 주행 경로가 겹치지 않게             |
|열악한 날씨                   |눈,비로 인해 마찰이 낮아지는 경우 제동거리 변화 고려 (적응형 주행전략)|




# Path generation
* * *
## Definition

# Path following
* * *
##