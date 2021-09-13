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

근데 명령창에서 이렇게 하는건 된다. idle에선 안되고....
```py
carrot@carrot-15U560-KA5MK:~$ python
Python 2.7.17 (default, Feb 27 2021, 15:10:58) 
[GCC 7.5.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy as np
>>> import matplotlib.pyplot as plt
>>> a=[1,2,3,4,5,6,7]
>>> print(a)
[1, 2, 3, 4, 5, 6, 7]
>>> plt.plot(a)
[<matplotlib.lines.Line2D object at 0x7f466dbe3710>]
>>> plt.show()
```
![스크린샷, 2021-09-13 16-25-40](https://user-images.githubusercontent.com/69246778/133041389-8a6be9ae-2ee5-4688-b722-366d2063a35f.png)

[서칭](https://ddangeun.tistory.com/104)하다가 python3에서 모듈 설치할 때 pip3를 사용한단 걸 보고 해봤다
```
carrot@carrot-15U560-KA5MK:~$ pip3 install matplotlib

Command 'pip3' not found, but can be installed with:

sudo apt install python3-pip

carrot@carrot-15U560-KA5MK:~$ sudo apt install python3-pip
```
![스크린샷, 2021-09-13 16-36-33](https://user-images.githubusercontent.com/69246778/133043185-c3c79f7c-e826-4f68-92c2-358c1f534c76.png)
그리고 나서 모듈을 다시 설치했더니

![스크린샷, 2021-09-13 16-37-12](https://user-images.githubusercontent.com/69246778/133043230-c0bceb7c-714b-4fb7-955a-47b52bfa9434.png)
설치가 되고 idle에서도 실행이 잘 된다.

![스크린샷, 2021-09-13 16-39-47](https://user-images.githubusercontent.com/69246778/133043340-28e506d8-eb43-48c3-97e6-e0344f146500.png)

