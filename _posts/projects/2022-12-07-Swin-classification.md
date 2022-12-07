---
layout: post
title: 
description: |
  Swin Transformer for Classification - Inference with ImageNet1K(mini version) dataset
hide_image: true
tags:
  - projects
published: true
---

# Swin Transformer for Classification - Inference with ImageNet1K(mini version) dataset
* * *

# 1. Training
```
python -m torch.distributed.launch --nproc_per_node 4 --master_port 12345  main.py \
--cfg configs/swin/swin_tiny_patch4_window7_224.yaml --data-path imagenet --batch-size 128
```
