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
 자동차는 수십 개의 ECU(전자 제어 장치)가 네트워크를 통해 연결되어 있는 매우 복잡하게 구성된 **Distributed Hard Real-Time Embedded System**이다. 센서에서 액추에이터까지 ECU와 네트워크로 연결되는데 **end-to-end delays**가 시간 정확성을 정의한다. **end-to-end delays(종단간지연)** 은 시작부터 목적지까지 통신하는데 걸리는 시간을 의미한다.   
 자동차의 구조를 단순화하면 다음과 같다.   
 ![image](https://user-images.githubusercontent.com/69246778/195063437-81a24e36-daf7-4281-8fab-96877903cde2.png)   
 왼쪽 부분에 있는 ECU묶음과 오른쪽 부분의 ECU묶음이 **Gateway**를 통해 연결되어 있는데, 이는 모든 ECU를 한 번에 작동시킬 수 없으므로
 필요한 ECU만 작동하도록 하는 역할이다.또한 많은 ECU가 **BUS**로 연결되어 있는데 이는 하나의 큰 줄기에 가지처럼 연결한 구조를 이야기한다.

# 4. Various Automotive Computing Platform
|기능|예시|특징|
|----|---|----|
|**Control**|(Multicore)CPU,CAN |- 기능적으로 안정적(ISO 26262,자동차 기능안전 표준을 따름)|   
|       |                   |- 실시간 스케줄링                                       |
|       |                   |- 멀티코어 검증/최적화                                   |
|**Infotainment(정보와 오락)**|CPU+GPU,Wireless(LTE,BT,...)|-보안(ISO/SAE 21434,자동차사이버보안표준)|
|                         |                           |-연결성(OTA,Over-The-Air:자동차에 내장된 소프트웨어를 무선으로 수정,추가,삭제하는 업데이트)|
|                         |                           |- 신규사용자경험(NUX,New Users Experiences)|
|**Intelligence**|CPU+GPU+NPU,Ethernet|-가속기(CPU계산 빨라지게..)|
|            |                    |- Intended안전(SOTIF,사람의 실수나 외부 요인에 의한 센서한계에 대한 표준|
|            |                    |- Choice of Operating System|


