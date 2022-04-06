---
layout: post
title: 
description: |
  Attention 구조 
hide_image: true
tags:
  - deeplearning
published: false
---

# (Python) The Attention Mechanism
* * *

```py
import numpy as np
from scipy.special import softmax  #출력층에서 softmax함수 사용


print("Step 1: 3 inputs, d_model=4")
x =np.array([[1.0, 0.0, 1.0, 0.0],   # Input 1
             [0.0, 2.0, 0.0, 2.0],   # Input 2
             [1.0, 1.0, 1.0, 1.0]])  # Input 3
print(x)
```
