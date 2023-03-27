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
   
# 2. Run
@ ~/바탕화면/VILS_DY/OSC-ALKS-scenarios/Scenarios
```
/home/carrot/바탕화면/VILS_DY/esmini/bin/esmini --window 60 60 800 400 --osc ALKS_Scenario_4.6_2_LateralDetectionRange_TEMPLATE.xosc
```
![image](https://user-images.githubusercontent.com/69246778/227835748-3f2035fc-aa5f-4903-9b2e-5bfa23668ae7.png)
   
```
/home/carrot/바탕화면/VILS_DY/esmini/bin/esmini --window 60 60 800 400 --osc ./resources/xosc/ALKS_Scenario_4.6_2_LateralDetectionRange_TEMPLATE.xosc --road_features on
```
![image](https://user-images.githubusercontent.com/69246778/227835999-68245007-c48c-4ba8-90f0-f9c74401ae78.png)

# 3. Examples
### 3.1. ALKS_Scenario_4.1_1_FreeDriving_TEMPLATE.xosc
### 3.2. ALKS_Scenario_4.1_2_SwervingLeadVehicle_TEMPLATE.xosc
### 3.3. ALKS_Scenario_4.1_3_SideVehicle_TEMPLATE.xosc
### 3.4. ALKS_Scenario_4.2_1_FullyBlockingTarget_TEMPLATE.xosc
### 3.5. ALKS_Scenario_4.2_2_PartiallyBlockingTarget_TEMPLATE.xosc
### 3.6. ALKS_Scenario_4.2_3_CrossingPedestrian_TEMPLATE.xosc
### 3.7. ALKS_Scenario_4.2_4_MultipleBlockingTargets_TEMPLATE.xosc
### 3.8. ALKS_Scenario_4.3_1_FollowLeadVehicleComfortable_TEMPLATE.xosc
### 3.9. ALKS_Scenario_4.3_2_FollowLeadVehicleEmergencyBrake_TEMPLATE.xosc
### 3.10. ALKS_Scenario_4.4_1_CutInNoCollision_TEMPLATE.xosc
### 3.11. ALKS_Scenario_4.4_2_CutInUnavoidableCollision_TEMPLATE.xosc
### 3.12. ALKS_Scenario_4.5_1_CutOutFullyBlocking_TEMPLATE.xosc
### 3.13. ALKS_Scenario_4.5_2_CutOutMultipleBlockingTargets_TEMPLATE.xosc
### 3.14. ALKS_Scenario_4.6_1_ForwardDetectionRange_TEMPLATE.xosc
### 3.15. ALKS_Scenario_4.6_2_LateralDetectionRange_TEMPLATE.xosc
