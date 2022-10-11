---
layout: post
title: 
description: |
  Real-Time Embedded Systems- 2. System Model
hide_image: true
tags:
  - autonomous
published: true
---

# Real-Time Embedded Systems- 2. System Model
* * *
대학원 강의 복습 및 정리용.

# 1. Real-Time embedded System
 **Real-Time System**이란, 논리적 정확성과 시간적 정확성을 모두 고려한 컴퓨텅 시스템을 말한다. 논리적 정확성이란 올바른 결과를 출력하는 것, 시간적 정확성이란 알맞은 시간에 결과를
출력하는 것을 의미한다. **Embedded System**은, 큰 시스템의 일부로서 특정 기능을 수행한다. 사용자들에게 잘 안보이도록 숨겨져 있는게 특징이다. Embedded System은 Real-Time system
이고, Real-Time System은 Embedded System인 것이 보통이다.   
 앞서, 임베디드 시스템은 **특정 기능을 수행한다**고 했는데 이것을 범용적인 목적을 갖는 컴퓨터와 비교하면 다음과 같다.
|Real-Time Embedded System|General Purpose Computer|
|-------------------------|------------------------|
| 특별한 목적을 가짐| 일반적인 목적을 가짐|
| 프로그래밍할 수 없음(고정적인 태스크만 수행함)|프로그래밍을 통해 유동적인 업무 수행|
| 물리적 세계와 상호작용 | 물리적 세계와의 상호작용 적음|
| 실시간성 필요|가능한 빠르게|
| 리소스가 제한적임.크기가 작아야한다거나, 배터리가 필요하다거나 등등| 지원되는게 많음|
| 하드웨어에 가까운 수준의 로우레벨 프로그래밍|하드웨어와 거리가 먼 하이레벨의 프로그래밍|   
   
# 2. Example System
 시스템은 크게 **Soft Real-Time Systems**과 **Hard Real-time Systems**으로 나뉜다. **Soft Real-Time**은 한계 시간을 초과해도 중대한 문제가 생기지 않는 시스템으로 *핸드폰,
공유기, 현관문시스템* 같은 것들이다. **Hard Real-Time**은 제약 조건을 한 번 이라도 만족하지 못하면 큰 문제가 야기되는 *공항 관제 시스템,자동차, 인공위성 발사* 등과 같은 것이다. 

# 3. Automotive System
 자동차는 매우 복잡하게 구성된 **Distributed Hard Real-Time Embedded System**이다. 수십 개의 ECU(전자 제어 장치)가 네트워크를 통해 연결되어 있고 센서부터 액추에이터까지
 ECU와 네트워크르
