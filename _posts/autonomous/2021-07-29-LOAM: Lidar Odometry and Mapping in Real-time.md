---
layout: post
title: Semminar2 - SLAM paper 
description: |
  
tags:
  - autonomous
use_math : true
comments : true
author: Udayeon

published: true
---

# (Review) LOAM: Lidar Odometry and Mapping in Real-time

[LOAM: Lidar Odometry and Mapping in Real-time](https://ieeexplore.ieee.org/document/7995716/metrics#metrics)   
- **Author** J. Zhang and S. Singh
- Robotics: Science and Systems Conference (RSS), July 2014.
{:.message}

**논문 선정 기준**   
* Camera SLAM이 아닌 것
* 인용 횟수 많은 것
* 분량이 적당한 것   
SLAM에 관한 (얕지만...)넓은 이해를 돕기 위해 실습에서 다루게 될 Camera SLAM이 아닌 다른 SLAM 관련 논문을 리뷰하고자 함.
구글 학술검색 기준 1084회 인용 되었음.

  - [Abstract]
  - [1.Introduction]
  - [2. Related work]
  - [3. Notation and task description]
  - [4. System overview]
  - [5. Lidar odometry]
  - [6. Lidar Mapping]
  - [7. Experiments]
  - [8. Conclusion and future work]

# Abstract
[6-DOF]로 움직이는 [2축 Lidar]의 range 측정을 이용한 odometry와 mapping의 real-time method를 제안.
range측정 값은 수신되는 시간이 다르고 motion estimation의 에러가 Point cloud 결과에 [misregistration]을 야기 하므로 어려운 문제임.
지금까지는 오프라인 [batch method]로 일관성 있는 3D map을 구축할 수 있었고 [loop closure]를 이용해 시간에 따른 [drift]를 줄였음.
제안할 method는 높은 정확도의 범위나 inertial(관성)의 측정이 필요없는 low-drift, low-computational complexity한 방법임. 
이러한 성능을 얻을 수 있는 핵심적인 아이디어는 SLAM의 복잡한 문제를 두개의 알고리즘으로 나누는 것임.
하나는 Lidar의 velocity 측정을 위해 high frequency, low fidelity에서 odometry를 수행. 다른 하나는, Point cloud의 fine matching과
registration을 위해 더 낮은 magnitude의 frequency를 수행. 두 알고리즘을 결합함으로써 실시간 mapping가능.
이 Method는 KITTI odometry benchmark뿐만 아니라 많은 실험에 의해 평가되어 왔음. 결과는 이 Method가 최첨단 오프라인 batch method
수준에서 정확성을 달성할 수 있음을 보여줌.

# 1. Introduction
3D mapping은 여전히 널리 사용되는 기술임. Lidar를 이용한 Maapping은 측정 거리에 상관없이 error가 상대적으로 일정한 high frquency의 
range 측정을 제공하므로 흔히 쓰이는 방식임. 레이저 빔 회전이 Lidar의 유일한 motion인 경우 Point Cloud의 register는 심플함. 
근데 Lidar자체가 움직일 때는 Lidar의 Pose에 대한 정보가 필요함. 이 문제를 해결하기 위한 일반적인 방법 중 하나는 독자적인 
Position estimation을 사용해 레이저 포인트를 고정된 좌표계에 register하는 것임. 또 다른 방법은, wheel encoder나 
Visual odometry system과 같은 odometry measurement를 사용하여 레이저 포인트를 register하는 것. odometry는 시간에 따른 
작은 incremental한 움직임을 통합하기 때문에 drift가 발생할 수 있으며 drift 감소에 집중해야함.   
   
여기서 고려하는 것은, 6-DOF에서 이동하는 2축 Lidar를 사용한 low-drift odometry를 통해 map을 제작하는 것.
Lidar사용의 주요 장점은 주변 불빛과 Scene의 optical texture에 예민하지 않다는 것임. 최근 Lidar의 발달로 크기와 무게가 줄었음.
Lidar는 환경을 traverse하는 다니거나 소형 항공기에도 탑재할 수 있음. 
본 방법은 odometry estimation에서의 drift를 최소화하는 것에 주목하고 있으므로 loop closure는 포함X.
   
이 method는 높은 정확도의 ranging이나 inertial의 측정 없이도 low-drift, low-computational complexity를 이룰 수 있음.
이러한 성능 수준을 달성하기 위한 핵심적인 아이디어는 다수의 변수를 동시에 최적화하는 복잡한 문제인 SLAM을, 
다음과 같은 두 개의 알고리즘으로 나누는 것.

|Lidar의 속도 측정|High frequency , Low fidelity|
|:---------------|:----------------------------|
|Point Cloud의 fine matching and registration|Frequency of an order of magnitude lower|

필요하진 않지만, 만약 IMU를 사용한다면, high frequency mothon을 설명하기 위한 motion prior가 제공될 수 있음.
특히, 두 알고리즘은 sharp한 모서리 및 평면에 위치한 feature point를 extract할 수 있고, 그 feature point를 각각 edge line segment와 
평면 패치에 match시킬 수 있음. Odometry 알고리즘에서는, 빠른 계산을 통해 feature point의 대응관계가 얻어지고
mapping알고리즘에서는, 고유값 및 고유벡터에 의한 Local point cluster의 기하학적 분산으로부터 대응관계가 얻어짐. 
   
original한 문제를 분석해 online motion estimation같은 보다 더 쉬운 문제가 해결됨. 그 후, mapping은 batch optimization으로써 
수행되어 고정밀 motion추정과 map을 생성. 이 parallel한 알고리즘 구조는, 실시간으로 문제해결을 가능케 하고 게다가, motion추정이
더 높은 frequency에서 수행되므로 mapping은 정확도를 위해 충분한 시간이 주어짐. 더 낮은 frequency에서 실행할 떄, mapping 알고리즘은
많은 feature point를 포함할 수 있고 convergence를 위해 충분히 많은 반복을 수행함. 
   
# 2. Related work
Lidar는 robot navigation에서 유용한 range sensor임. localization과 mapping을 위해 대부분의 application은 2D Lidar를 사용함. 
Lidar의 스캔 속도가 외부 움직임보다 빠를 때, 스캔에서 발생하는 움직임의 distortion은 무시되는 경우가 많음
이러한 경우에, Standard ICP method를 사용해 다른 scan간의 laser return을 match 시킬 수 있고 왜곡을 제거하기 위해 Two-step method가
제안됨. velocity를 ICP로 추정하고 계산된 velocity로 distortion을 보상해줌. 비슷한 기술은 단축 3D Lidar에 의한 distortion을 
보상하는데에 쓰임. 그러나, 만약 scanning 속도가 상대적으로 느리다면, motion distortion이 극심할 수 있음. 
이는 특히 2축 Lidar에서 발생하는데, 2축 라이다의 한쪽 축이 다른 쪽 축보다 훨씬 느리기 때문. 
종종, 다른 센서를 사용해 속도를 추정해 distortion을 제거할 수 있음. 예를  들어, Lidar cloud는 IMU와 통합된 Visual Odometry에 의한
상태 추정으로부터 register될 수 있음. GPS/INS같은 여러 센서가 동시에 사용될 경우 확장된 Kalman filter나 Particle filter로 문제가 
해결됨. 이러한 방법은 실시간으로 지도를 만들어 path planning과 충돌 회피를 도움. 
   
다른 센서의 도움없이 2축 Lidar를 사용한다면 motion 추정과 distortion의 보정이 문제가 됨. 이를 해결하는 Barfoot등에 의해 사용된 method는
laser intensity return으로 부터 visual image를 만들고, 이미지 간의 시각적으로 구별되는 features를 match하여 groud vehicle의 
motion을 다루는 방법임. vehicle의 motion 은 일정한 속도로 모델링되고 Gaussian process를 사용함. 본 논문의 method는 visual odometry
알고리즘에서 이와 비슷한 선형 운동 모델을 사용하지만, feature의 유형이 다름. 전자는 intensity image의 visual feature와
밀집된 point cloud가 필요함. 후자(out method)는 데카르트 공간에서의 기하학적 feature를 extract하고 matching하며, cloud의 밀도가
낮아도 ㄱㅊ.
   
우리의 approach는 Bosse와 Zlot이랑 가장 유사함. 그들은 2축 Lidar를 사용해 local point cluster의 기하학적 구조를 매칭시켜서 
point cloud를 register함. 게다가, 그들은 다수의 2축 Lidar를 사용해 underground mine을 지도화함. 이 방법은 IMU를 포함하고 있고
loop closure를 이용해 거대한 map을 만드는 것임. 같은 저자가 제안한 **Zebedee**는 spring으로 hand-bar에 부착된  IMU와 2D Lidar로
구성된 mapping 장치임. Mapping은 이 장치를 손으로 움직여 수행함. Trajectory는 batch optimization method로 recover되는데, 
이는 segment간에 constraint가 추가된 dataset을 처리하는 방법임. 이 method에서, IMU의 측정은 레이저 포인트를 register하고 
IMU의 biasd를 수정하는데 사용. 본질적으로, Bosse와 Zlot의 방법은, 정확한 지도를 위한 batch processing이 필요하므로 실시간으로
지도가 필요한 application에는 부적합. 이에 대해, 제안된 실시간성의 method는 질적으로 Bosse와 Zlot의 method와 유사핟.
차이점은, 우리의 method가 자율주행차량 안내를 위한 motion 추정을 제공한다는 것이다. 게다가, 이 방법은 Lidar scan pattern과
point cloud 분산을 사용한다. Feature matching은 odometry와 mapping알고리즘에서의 계산속도와 정확도를 보장한다.

# 5. Lidar odometry
Lidar cloud P_k에서 feature를 extraction하는 것부터 시작. Lidar는 자연스럽게 P_k에 불균일한 point를 생성.
레이저 스캐너의 return은 스캔 내에서  0.25◦ 의 resolution을 가짐. 이 point들은 scan plane에 위치함.
그러나, laser scanner가 180◦/s의 각속도로 회전하고 40Hz에서 scan을 생성할 때, scan plane의 수직방향 resolution은
180◦/40=4.5◦. 이러한 사실을 고려하면서, 개별적인 scan의 정보만을 이용하여  P_k로부터 feature point를 extract
