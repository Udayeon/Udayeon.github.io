---
layout: post
title: 
description: |
  OpenScenario Editor
hide_image: true
tags:
  - autonomous
published: true
---

# OpenScenario Editor
* * *
[git hub](https://github.com/ebadi/OpenScenarioEditor)   
   
# 1. insatll
![image](https://user-images.githubusercontent.com/69246778/220040352-49b9d0bc-ad10-462b-a43b-50cae12d8475.png)   
   
# 2. run
```
carrot@carrot-MS-7D17:~/바탕화면/OpenScenarioEditor/OpenScenarioEditor$ ./run.sh
```
```
File - Open Scenario - pyesmini - resources - xosc
```
![image](https://user-images.githubusercontent.com/69246778/220041907-cf204a68-022f-4dba-8ed9-253263b6f46b.png)   
![image](https://user-images.githubusercontent.com/69246778/220041958-4a8c9a1e-3ddd-4fe1-be9b-cf0a3010929d.png)   

# 3. Example1. cut-in.xosc (e6mini.xodr)
## 3.1. Road
### track.csv
@ /VILS_DY/OpenScenarioEditor/pyesmini
```
./bin/odrplot ./resources/xodr/e6mini.xodr
```
### plot track.csv
```
python ./EnvironmentSimulator/Applications/odrplot/xodr.py track.csv
```
* **Error**
```
Traceback (most recent call last):
  File "./EnvironmentSimulator/Applications/odrplot/xodr.py", line 6, in <module>
    import matplotlib.pyplot as plt
ImportError: No module named matplotlib.pyplot
```
   
pip install matplotlib해도 계속 같은 에러가 발생해서 Colab환경에서 진행함.
```
from google.colab import drive
drive.mount('/content/drive')
```
```
cd drive/MyDrive/VILS/
```
```
import sys
import csv
import math
import matplotlib.pyplot as plt

H_SCALE = 10

with open('track.csv') as f:
    reader = csv.reader(f, skipinitialspace=True)
    positions = list(reader)

ref_x = []
ref_y = []
ref_z = []
ref_h = []

lane_x = []
lane_y = []
lane_z = []
lane_h = []

border_x = []
border_y = []
border_z = []
border_h = []

for pos in positions:
    if pos[0] == 'lane':
        if pos[3] == '0':
            ltype = 'ref'
            ref_x.append([])
            ref_y.append([])
            ref_z.append([])
            ref_h.append([])
        elif pos[4] == 'no-driving':
            ltype = 'border'
            border_x.append([])
            border_y.append([])
            border_z.append([])
            border_h.append([])
        else:
            ltype = 'lane'
            lane_x.append([])
            lane_y.append([])
            lane_z.append([])
            lane_h.append([])
    else:
        if ltype == 'ref':
            ref_x[-1].append(float(pos[0]))
            ref_y[-1].append(float(pos[1]))
            ref_z[-1].append(float(pos[2]))
            ref_h[-1].append(float(pos[3]) + math.pi/2.0)
        elif ltype == 'border':
            border_x[-1].append(float(pos[0]))
            border_y[-1].append(float(pos[1]))
            border_z[-1].append(float(pos[2]))
            border_h[-1].append(float(pos[3]) + math.pi/2.0)
        else:
            lane_x[-1].append(float(pos[0]))
            lane_y[-1].append(float(pos[1]))
            lane_z[-1].append(float(pos[2]))
            lane_h[-1].append(float(pos[3]) + math.pi / 2.0)

p1 = plt.figure(1)

for i in range(len(ref_x)):
    plt.plot(ref_x[i], ref_y[i], linewidth=2.0, color='#BB5555')
for i in range(len(lane_x)):
    plt.plot(lane_x[i], lane_y[i], linewidth=1.0, color='#3333BB')
for i in range(len(border_x)):
    plt.plot(border_x[i], border_y[i], linewidth=1.0, color='#AAAAAA')

# hdg_lines = []
# for i in range(len(h)):
#     for j in range(len(h[i])):
#         hx = x[i][j] + H_SCALE * math.cos(h[i][j])
#         hy = y[i][j] + H_SCALE * math.sin(h[i][j])
#         plt.plot([x[i][j], hx], [y[i][j], hy])

plt.gca().set_aspect('equal', 'datalim')

#p2 = plt.figure(2)
#for i in range(len(z)):
#    ivec = [j for j in range(len(z[i]))]
#    plt.plot(z[i])

plt.show()
```
![image](https://user-images.githubusercontent.com/69246778/224246804-1bc3b3a8-bfa3-4d1a-a40f-8ea852b91da4.png)

### Road network visualization
```
./bin/esmini --window 60 60 800 400 --osc ./resources/xosc/cut-in.xosc --road_features on
```
![image](https://user-images.githubusercontent.com/69246778/224248429-8cbd4d41-73a1-40e4-b206-efb3aba3f0b4.png)


### The basic text log file
```
./bin/esmini --window 60 60 800 400 --osc ./resources/xosc/slow-lead-vehicle.xosc --disable_log
```
```
esmini GIT REV: b772909
esmini GIT TAG: 
esmini GIT BRANCH: tags/v2.2.0~8
esmini BUILD VERSION: N/A - client build
ScenarioEngine.cpp / 35 / InitScenario(): Init ./resources/xosc/cut-in.xosc
ScenarioReader.cpp / 57 / loadOSCFile(): Loading ./resources/xosc/cut-in.xosc
Parameters.cpp / 243 / parseParameterDeclarations(): Parsing ParameterDeclarations
ScenarioReader.cpp / 216 / parseRoadNetwork(): Parsing RoadNetwork
ScenarioReader.cpp / 247 / parseRoadNetwork(): Roadnetwork ODR: ../xodr/e6mini.xodr
ScenarioReader.cpp / 248 / parseRoadNetwork(): Scenegraph: ../models/e6mini.osgb
RoadManager.cpp / 4451 / SetRoadOSI(): Generating OSI lanes
RoadManager.cpp / 4453 / SetRoadOSI(): Generating OSI road marks
RoadManager.cpp / 4455 / SetRoadOSI(): Generating OSI lane boundaries
RoadManager.cpp / 4457 / SetRoadOSI(): OSI road features done
ScenarioReader.cpp / 592 / parseCatalogs(): Parsing Catalogs
ScenarioReader.cpp / 742 / parseEntities(): Parsing Entities
Parameters.cpp / 56 / getParameter(): Resolve parameter $HostVehicle
Parameters.cpp / 64 / getParameter(): $HostVehicle replaced with car_white
ScenarioReader.cpp / 173 / LoadCatalog(): Loading catalog VehicleCatalog
ScenarioReader.cpp / 345 / parseOSCVehicle(): Parsing Vehicle car_white
ScenarioReader.cpp / 263 / ParseOSCProperties(): Properties/File = ../models/car_white.osgb registered
ScenarioReader.cpp / 272 / ParseOSCProperties(): Property model_id = 0 registered
Parameters.cpp / 56 / getParameter(): Resolve parameter $TargetVehicle
Parameters.cpp / 64 / getParameter(): $TargetVehicle replaced with car_red
ScenarioReader.cpp / 345 / parseOSCVehicle(): Parsing Vehicle car_red
ScenarioReader.cpp / 263 / ParseOSCProperties(): Properties/File = ../models/car_red.osgb registered
ScenarioReader.cpp / 272 / ParseOSCProperties(): Property model_id = 2 registered
ScenarioReader.cpp / 1935 / parseInit(): Parsing init
Parameters.cpp / 56 / getParameter(): Resolve parameter $EgoStartS
Parameters.cpp / 64 / getParameter(): $EgoStartS replaced with 50
Parameters.cpp / 234 / ReadAttribute(): Warning: missing required attribute: value
ScenarioReader.cpp / 2666 / parseStoryBoard(): Parsing Story
Story.cpp / 21 / Story(): Story: New Story CutInAndBrakeStory created
Parameters.cpp / 243 / parseParameterDeclarations(): Parsing ParameterDeclarations
Parameters.cpp / 56 / getParameter(): Resolve parameter $owner
Parameters.cpp / 64 / getParameter(): $owner replaced with OverTaker
ScenarioReader.cpp / 2551 / parseOSCManeuver(): Parsing OSCManeuver CutInManeuver
ScenarioReader.cpp / 2566 / parseOSCManeuver(): Parsing Event OverTakerStartSpeedEvent
ScenarioReader.cpp / 2623 / parseOSCManeuver(): Parsing private action OverTakerStartSpeedAction
ScenarioReader.cpp / 2529 / parseTrigger(): Parsing Trigger
ScenarioReader.cpp / 2121 / parseOSCCondition(): Parsing OSCCondition OverTakerStartSpeedCondition
ScenarioReader.cpp / 2566 / parseOSCManeuver(): Parsing Event CutInEvent
ScenarioReader.cpp / 2623 / parseOSCManeuver(): Parsing private action CutInAction
ScenarioReader.cpp / 2529 / parseTrigger(): Parsing Trigger
ScenarioReader.cpp / 2121 / parseOSCCondition(): Parsing OSCCondition CutInStartCondition
Parameters.cpp / 56 / getParameter(): Resolve parameter $owner
Parameters.cpp / 64 / getParameter(): $owner replaced with OverTaker
Parameters.cpp / 56 / getParameter(): Resolve parameter $HeadwayTime_LaneChange
Parameters.cpp / 64 / getParameter(): $HeadwayTime_LaneChange replaced with 0.4
ScenarioReader.cpp / 2566 / parseOSCManeuver(): Parsing Event OvertakerBrakeEvent
ScenarioReader.cpp / 2623 / parseOSCManeuver(): Parsing private action OvertakerBrakeAction
ScenarioReader.cpp / 2529 / parseTrigger(): Parsing Trigger
ScenarioReader.cpp / 2121 / parseOSCCondition(): Parsing OSCCondition BrakeCondition
Parameters.cpp / 56 / getParameter(): Resolve parameter $owner
Parameters.cpp / 64 / getParameter(): $owner replaced with OverTaker
Parameters.cpp / 56 / getParameter(): Resolve parameter $HeadwayTime_Brake
Parameters.cpp / 64 / getParameter(): $HeadwayTime_Brake replaced with 0.7
ScenarioReader.cpp / 2529 / parseTrigger(): Parsing Trigger
ScenarioReader.cpp / 2121 / parseOSCCondition(): Parsing OSCCondition CutInActStart
ScenarioReader.cpp / 2529 / parseTrigger(): Parsing Trigger
ScenarioReader.cpp / 2121 / parseOSCCondition(): Parsing OSCCondition ActStopCondition
Entities.hpp / 402 / Print(): 
Story.cpp / 135 / Print(): Storyboard:
Story.cpp / 88 / Print(): Story: 
0.000 OSCPrivateAction.cpp / 703 / Start(): Ego pos: 
0.000 RoadManager.cpp / 6149 / Print(): Position:
0.000 RoadManager.cpp / 6134 / PrintTrackPos(): 	Track pos: (road_id 0, s 50.00, t -8.00, h 1.57)
0.000 RoadManager.cpp / 6139 / PrintLanePos(): 	Lane pos: (road_id 0, lane_id -3, s 50.00, offset 0.00, h 1.57)
0.000 RoadManager.cpp / 6144 / PrintInertialPos(): 	Inertial pos: (x 8.17, y 49.97, z -0.04, h 1.57, p 0.00, r 0.00)
0.000 OSCAction.cpp / 92 / SetState(): Init Ego TeleportAction standbyState -> startTransition -> runningState
0.000 OSCPrivateAction.cpp / 703 / Start(): OverTaker pos: 
0.000 RoadManager.cpp / 6149 / Print(): Position:
0.000 RoadManager.cpp / 6134 / PrintTrackPos(): 	Track pos: (road_id 0, s 25.00, t -4.42, h 1.57)
0.000 RoadManager.cpp / 6139 / PrintLanePos(): 	Lane pos: (road_id 0, lane_id -2, s 25.00, offset 0.00, h 1.57)
0.000 RoadManager.cpp / 6144 / PrintInertialPos(): 	Inertial pos: (x 4.51, y 24.98, z -0.01, h 1.57, p 0.00, r 0.00)
0.000 OSCAction.cpp / 92 / SetState(): Init OverTaker TeleportAction standbyState -> startTransition -> runningState
0.000 OSCAction.cpp / 92 / SetState(): Init Ego TeleportAction runningState -> stopTransition -> completeState
0.000 OSCAction.cpp / 92 / SetState(): Init OverTaker TeleportAction runningState -> stopTransition -> completeState
0.000 ScenarioGateway.cpp / 207 / reportObject(): Creating new object "Ego" (id 0, timestamp 0.00)
0.000 ScenarioGateway.cpp / 207 / reportObject(): Creating new object "OverTaker" (id 1, timestamp 0.00)
0.000 viewer.cpp / 716 / Viewer(): Anti-Aliasing number of subsamples: 4
0.000 viewer.cpp / 588 / CarModel(): No wheel nodes in model OverTaker. No problem, wheels will just not appear to roll or steer
0.010 OSCCondition.cpp / 237 / Evaluate(): Trigger /------------------------------------------------
0.010 OSCCondition.cpp / 407 / Log(): CutInActStart == true, 0.0100 > 0.00 edge: NONE
0.010 OSCCondition.cpp / 253 / Evaluate(): Trigger  ------------------------------------------------/
0.010 OSCAction.cpp / 92 / SetState(): CutInAndBrakeAct standbyState -> startTransition -> runningState
0.010 OSCCondition.cpp / 237 / Evaluate(): Trigger /------------------------------------------------
0.010 OSCCondition.cpp / 342 / Log(): OverTakerStartSpeedCondition == true, element: CutInAndBrakeAct state: START_TRANSITION, edge: NONE
0.010 OSCCondition.cpp / 253 / Evaluate(): Trigger  ------------------------------------------------/
0.020 OSCAction.cpp / 92 / SetState(): OverTakerStartSpeedAction standbyState -> startTransition -> runningState
0.020 OSCAction.cpp / 92 / SetState(): OverTakerStartSpeedEvent standbyState -> startTransition -> runningState
6.190 OSCCondition.cpp / 237 / Evaluate(): Trigger /------------------------------------------------
6.190 OSCCondition.cpp / 511 / Log(): CutInStartCondition == true, HWT: 0.40 > 0.40, edge Rising
6.190 OSCCondition.cpp / 248 / Evaluate(): Triggering entity 0: Ego
6.190 OSCCondition.cpp / 253 / Evaluate(): Trigger  ------------------------------------------------/
6.190 ScenarioEngine.cpp / 279 / step(): Event OverTakerStartSpeedEvent ended, overwritten by event CutInEvent
6.207 OSCAction.cpp / 92 / SetState(): OverTakerStartSpeedAction runningState -> endTransition -> standbyState
6.207 OSCAction.cpp / 92 / SetState(): OverTakerStartSpeedEvent runningState -> endTransition -> standbyState
6.207 OSCAction.cpp / 92 / SetState(): CutInAction standbyState -> startTransition -> runningState
6.207 OSCAction.cpp / 92 / SetState(): CutInEvent standbyState -> startTransition -> runningState
7.690 OSCCondition.cpp / 237 / Evaluate(): Trigger /------------------------------------------------
7.690 OSCCondition.cpp / 511 / Log(): BrakeCondition == true, HWT: 0.70 > 0.70, edge Rising
7.690 OSCCondition.cpp / 248 / Evaluate(): Triggering entity 0: Ego
7.690 OSCCondition.cpp / 253 / Evaluate(): Trigger  ------------------------------------------------/
7.690 ScenarioEngine.cpp / 307 / step(): Event(s) ongoing, OvertakerBrakeEvent will run in parallel
7.706 OSCAction.cpp / 92 / SetState(): OvertakerBrakeAction standbyState -> startTransition -> runningState
7.706 OSCAction.cpp / 92 / SetState(): OvertakerBrakeEvent standbyState -> startTransition -> runningState
9.190 OSCAction.cpp / 92 / SetState(): CutInAction runningState -> endTransition -> standbyState
9.190 OSCAction.cpp / 92 / SetState(): CutInEvent runningState -> endTransition -> standbyState
16.691 OSCCondition.cpp / 200 / Evaluate(): ActStopCondition timer 5.00s started
16.691 OSCAction.cpp / 92 / SetState(): OvertakerBrakeAction runningState -> endTransition -> standbyState
16.691 OSCAction.cpp / 92 / SetState(): OvertakerBrakeEvent runningState -> endTransition -> standbyState
21.692 OSCCondition.cpp / 184 / Evaluate(): ActStopCondition timer expired at 5.00 seconds
21.692 OSCCondition.cpp / 237 / Evaluate(): Trigger /------------------------------------------------
21.692 OSCCondition.cpp / 342 / Log(): ActStopCondition == true, element: OvertakerBrakeEvent state: END_TRANSITION, edge: Rising
21.692 OSCCondition.cpp / 253 / Evaluate(): Trigger  ------------------------------------------------/
21.692 OSCAction.hpp / 131 / End(): CutInAndBrakeAct complete after 1 execution
21.692 OSCAction.cpp / 92 / SetState(): CutInAndBrakeAct runningState -> endTransition -> completeState
21.692 ScenarioEngine.cpp / 411 / step(): All acts are done, quit now
ScenarioEngine.cpp / 90 / ~ScenarioEngine(): Closing
```

### CSV logger
```
./bin/esmini --window 60 60 800 400 --osc ./resources/xosc/cut-in.xosc --fixed_timestep 0.05 --csv_logger full_log.csv
```

### Scenario recording (.dat)
```
./bin/esmini --window 60 60 800 400 --osc ./resources/xosc/slow-lead-vehicle.xosc --fixed_timestep 0.05 --record sim.dat
```
.dat to .csv
```
./bin/dat2csv sim.dat
```
