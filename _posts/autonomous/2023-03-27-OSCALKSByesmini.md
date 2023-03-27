---
layout: post
title: 
description: |
  Open the OSC-ALKS-scenarios by esmini
hide_image: true
tags:
  - autonomous
published: true
---

# 1. Settings
* * *
@ local terminal 
* **OSC-ALKS-scenarios**
```
git clone https://github.com/asam-oss/OSC-ALKS-scenarios.git
```
* **esmini**
[Install latest esmini](https://github.com/esmini/esmini/releases)   
   
# 2. run
@ ~/바탕화면/VILS_DY/OSC-ALKS-scenarios/Scenarios
```
/home/carrot/바탕화면/VILS_DY/esmini/bin/esmini --window 60 60 800 400 --osc ALKS_Scenario_4.6_2_LateralDetectionRange_TEMPLATE.xosc
```
![image](https://user-images.githubusercontent.com/69246778/227835748-3f2035fc-aa5f-4903-9b2e-5bfa23668ae7.png)
   
```
/home/carrot/바탕화면/VILS_DY/esmini/bin/esmini --window 60 60 800 400 --osc ./resources/xosc/ALKS_Scenario_4.6_2_LateralDetectionRange_TEMPLATE.xosc --road_features on
```
![image](https://user-images.githubusercontent.com/69246778/227835999-68245007-c48c-4ba8-90f0-f9c74401ae78.png)

