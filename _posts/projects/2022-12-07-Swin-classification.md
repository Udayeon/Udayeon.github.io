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

# 1. dataset
[ImageNet 1k Mini Ver](https://www.kaggle.com/datasets/ifigotin/imagenetmini-1000?resource=download)

# 1. Training
```
python -m torch.distributed.launch --nproc_per_node 4 --master_port 12345  main.py \
--cfg configs/swin/swin_tiny_patch4_window7_224.yaml --data-path imagenet --batch-size 128
```

# 2. Evaluation
```
python -m torch.distributed.launch --nproc_per_node 1 --master_port 12345 main.py --eval \
--cfg configs/swin/swin_tiny_patch4_window7_224.yaml --resume output/swin_tiny_patch4_window7_224_swin2/default/ckpt_epoch_268.pth --data-path imagenet
```

![image](https://user-images.githubusercontent.com/69246778/206333425-13244d1b-d5f3-44ad-997a-a4f1d7133e09.png)


