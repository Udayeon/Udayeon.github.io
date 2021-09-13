---
layout: post
title: (Python) matplotlib설치
description: |
  matplotlib설치
hide_image: true
tags:
  - programming
published: true
---

# (Python) matplotlib설치
* * *
파이썬에서 그래프를 그리는데 필요한 라이브러리인 matplotlib설치하기

## 문제상황
```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import cv2, numpy as np
import matplotlib.pyplot as plt


grey = cv2.imread('../data/Lena.png',0)
cv2.imshow('original grey',grey)
cv2.waitKey()
cv2.destroyAllWindows()
```
opencv 학습 중에 위와 같은 코드를 사용하는데,다음과 같은 오류가 발생한다.

```py
Traceback (most recent call last):
  File "/home/carrot/바탕화면/Opencv3ComputerVisionWithPythonCookbook/OpenCV3ComputerVisionwithPythonCookbook_Code/Chapter02/08 Computing image histogram.py", line 5, in <module>
    import matplotlib.pyplot as plt
ModuleNotFoundError: No module named 'matplotlib'
```
matplotlib가 설치되어 있지 않았다.

## 설치방법
터미널을 열고 다음과 같은 명령어를 입력
```
pip install matplotlib
```

![스크린샷, 2021-09-13 16-15-41](https://user-images.githubusercontent.com/69246778/133040032-57fab71e-e0e5-445c-a08c-8b5137583399.png)
설치 된줄 알았는데 import해보면 여전히 오류가 발생함
```
>>> import matplotlib.pyplot as plt
Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    import matplotlib.pyplot as plt
ModuleNotFoundError: No module named 'matplotlib'
```
