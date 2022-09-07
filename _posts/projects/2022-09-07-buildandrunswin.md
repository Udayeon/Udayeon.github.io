---
layout: post
title: 
description: |
  Build and run Swin transformer
hide_image: true
tags:
  - projects
published: true
---

# Build and run Swin transformer
* * *


# 1. Install
![image](https://user-images.githubusercontent.com/69246778/188855240-07245ea1-02c4-4d71-a296-fda0dc63d511.png)
1. [(Ubuntu 20.04) How to get started with Docker](https://udayeon.github.io/2022/09/07/docker/)
2. [(Ubuntu 20.04) Using Docker containers in VS Code](https://udayeon.github.io/2022/09/07/dockerWithvscode/)

# 2. Clone this repo
```
git clone https://github.com/microsoft/Swin-Transformer.git
cd Swin-Transformer
```
![image](https://user-images.githubusercontent.com/69246778/188855585-34f613cb-55fb-432e-8b7c-fa0026afd197.png)


# 3. Create a conda virtual environment and activate it
```
conda create -n swin python=3.7 -y
conda activate swin
```

# 4. Install CUDA, cudnn, pytorch
```
conda install pytorch==1.8.0 torchvision==0.9.0 cudatoolkit=10.2 -c pytorch
```

# 5. Install timm
```
pip install timm==0.4.12
```

# 6. Install other requirements
```
pip install opencv-python==4.4.0.46 termcolor==1.1.0 yacs==0.1.8
```

# 7. Install fused window process for acceleration, activated by passing '--fused_window_process' in the running script
```
cd kernels/window_process
python setup.py install
```
