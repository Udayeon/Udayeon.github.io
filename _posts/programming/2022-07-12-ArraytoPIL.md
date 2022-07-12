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

```py
import numpy as np
from PIL import Image

image = Image.open("image.png") # 이 상태는 type이 <class 'PIL.PngImagePlugin.PngImageFile'> 이거임
np_array = np.array(image) # array로 바꿔줌

pil_image=Image.fromarray(np_array) # type = pil image
pil_image.show()
```

```py
image = cv2.imread('/content/image0.png', cv2.IMREAD_COLOR) # 이렇게 열면 type이 array
```
