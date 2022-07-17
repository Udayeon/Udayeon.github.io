---
layout: post
title: 
description: |
  (python) 폴더 내 파일이름 바꾸기
hide_image: true
tags:
  - programming
published: true
---

# (python) 특정 폴더 내 파일이름 바꾸고 저장하기
* * *
```py
import os
file_path = 'C:\\Users\\유다연\\Desktop\\image'
file_names = os.listdir(file_path)

i = 0
for name in file_names:
    src = os.path.join(file_path, name)
    dst = str('im') + str(i) + '.jpg'
    dst = os.path.join(file_path, dst)
    os.rename(src, dst)
    i += 1

```
