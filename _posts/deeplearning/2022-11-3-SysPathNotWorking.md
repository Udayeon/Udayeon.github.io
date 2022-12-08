---
layout: post
title: 
description: |
  ERROR : When 'sys.path.append()' not working

hide_image: true
tags:
  - deeplearning
published: True
---

# ERROR : When 'sys.path.append()' not working
* * *

# The problem
```
>>> import sys
>>> sys.path
['', '/opt/conda/lib/python37.zip', '/opt/conda/lib/python3.7', '/opt/conda/lib/python3.7/lib-dynload', '/opt/conda/lib/python3.7/site-packages', '/root/mmdetection', '/opt/conda/lib/python3.7/site-packages/swin_window_process-0.0.0-py3.7-linux-x86_64.egg']
```

```
>>> sys.path.append('/mmdetection/mmdetection')
>>> sys.path
>>> ['', '/opt/conda/lib/python37.zip', '/opt/conda/lib/python3.7', '/opt/conda/lib/python3.7/lib-dynload', '/opt/conda/lib/python3.7/site-packages', '/root/mmdetection', '/opt/conda/lib/python3.7/site-packages/swin_window_process-0.0.0-py3.7-linux-x86_64.egg', '/mmdetection/mmdetection']
```
But, If rerun 'sys.path', it doesn't apply
```
>>> import sys
>>> sys.path
['', '/opt/conda/lib/python37.zip', '/opt/conda/lib/python3.7', '/opt/conda/lib/python3.7/lib-dynload', '/opt/conda/lib/python3.7/site-packages', '/root/mmdetection', '/opt/conda/lib/python3.7/site-packages/swin_window_process-0.0.0-py3.7-linux-x86_64.egg']
```
