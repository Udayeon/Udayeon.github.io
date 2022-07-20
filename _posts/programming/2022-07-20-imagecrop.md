---
layout: post
title: 
description: |
  python으로 폴더 내 이미지 일괄적으로 crop하기
hide_image: true
tags:
  - programming
published: true
---

# (python) 특정 폴더 내 이미지를 일괄적으로 crop하고 저장하기
* * *
해당 script를 수정하고자 하는 이미지들이 담겨있는 폴더로 이동한 후 실행시킨다. 
```py
from PIL import Image
import os

file_path = 'C:\\Users\\유다연\\Desktop\\PixelLabelData' # PixelLabelData라는 폴더 내의 이미지를 처리
file_names = os.listdir(file_path)

i = 891
for name in file_names:
    img = Image.open(str('Label_')+str(i)+'.png') # 해당 폴더 내 파일명이 'Label_1.png'형식으로 되어 있음. 하나씩 연다.
    area = (0,0,640,174)  # 해당 영역만 남기고 나머지 잘라냄.
    cropped_img = img.crop(area)
    cropped_img.save(str('Label_')+str(i)+'.png')
    i+=1

```

