---
layout: post
title: 
description: |
  array를 PIL image로 변환
hide_image: true
tags:
  - programming
published: true
---

# (python) array를 PIL image로 변환하기
* * *

# 1. PIL.Image로 open
```py
import numpy as np
from PIL import Image

image = Image.open("image.png") # 이 상태는 type이 <class 'PIL.PngImagePlugin.PngImageFile'> 이거임
np_array = np.array(image) # array로 바꿔주는 과정 필요

pil_image=Image.fromarray(np_array) # type = pil image
pil_image.show()
```

# 2. cv2.imread로 open
```py
import cv2
image = cv2.imread('/content/image0.png', cv2.IMREAD_COLOR) # imread로 열면 type이 array

pil_image=Image.fromarray(image) # type = pil image
pil_image.show()
```
