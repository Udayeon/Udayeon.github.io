---
layout: post
title: 
description: |
  CSWin Transformer: A General Vision Transformer Backbone with Cross-Shaped Windows, 9 Jan 2022, University of Science and Technology China, Microsoft.
hide_image: true
tags:
  - review
published: true
---

# CSWin Transformer: A General Vision Transformer Backbone with Cross-Shaped Windows
[Link](https://arxiv.org/abs/2107.00652)
* * *
 A challenging issue in Transformer design is that **global self-attention is very expensive to compute, whereas local self-attention often limits the
field of interactions of eash token.** To adress this issue, we develop the **Cross-Shaped Winodow self-attention mechanism** for computing self-attention
in the horizontal and vertical stripes in parallel that form a cross-shaped window, with eace stripe obtained by splitting the input feature into stripes 
of equal width. We also introduce **Locally-enhanced Positional Encoding(LePE)**, which handles the local positional information better than existing 
encoding schemes.     
Results of **Cascade Mask R-CNN 3x + MS**
|Pretraining|Backbone|Dataset|Task|Results|
|-----------|--------|-------|---------|---------------------|
|ImageNet-1K|Swin-T  |COCO   |Detection|50.4boxAP, 43.7maskAP|
|           |Swin-S  |       |         |51.9boxAP, 45.0maskAP|
|           |Swin-B  |       |         |51.9boxAP, 45.0maskAP|
|           |CSwin-T |       |         |52.5boxAP, 45.3maskAP|
|           |CSwin-S |       |         |53.7boxAP, 46.4maskAP|
|           |CSwin-B |       |         |53.9boxAP, 46.4maskAP|


# 1. Introduction
 To improve the full-attention mechanism efficiency, one typical way is to limit the attention region of each token from full-attention to local/windowed
 attention.

## 1.1. 소제목
내용내용

## 1.2. 소제목
어쩔저쩔
