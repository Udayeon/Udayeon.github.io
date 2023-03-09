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
![image](https://user-images.githubusercontent.com/69246778/223963553-4db57e34-c373-4dce-9d77-625b4644e441.png)
```

```
