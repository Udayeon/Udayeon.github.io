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
![image](https://user-images.githubusercontent.com/69246778/192468426-8b397e6c-466d-4089-b0bc-320c2a9f95b8.png)   
 The Transformer architecture with full-attention mechanism is computationally inefficient. To improve the full-attention mechanism efficiency, one typical way is **to limit the attention region of each token from full-attention to local/windowed attention.** The key is to achieve large receptive filed efficiently while keeping the computation cost low.   
    
 This paper presents **Cross-Shaped Window self-attention.** with **CSwin self-attention**, we perform the self-attention calculation in the horizontal and vertical striped in **parallel**, with each stripe obtained by splitting the input feature into stripes of equal **width**(This stripe width is an important param).   
    
 It is worthwhile to note that with CSwin self-attention mechanism, **the self-attention in horizontal and vertical stripes are calculated in parallel.**
This strategy splits the multi-heads into parallel groups and applies different self-attention operations onto groups.   
   
 Based on the CSwin self-attention mechanism and hierarchical design, we propose a new vision Transformer architecture named **"CSwin Transformer"**. To further enhance this vision Transformer,
we introduce **Locally-enhanced Positional Encoding(LePE)** in this paper.


# 2. Related Work
## 2.1. Vision Transformer
## 2.2. Efficient Self-attentions
## 2.3. Positional Encoding

# 3. Method
## 3.1. Overall Architecture
![image](https://user-images.githubusercontent.com/69246778/192493775-faa94551-5c89-4d79-b8be-ddad15716310.png)   
**Stage1**   
**Input Image : H * W * 3**   
**Conv layer : 7 * 7, Stride 4**   
**Output Patch token : H/4 *  W/4 * C**   

## 3.2. Cross-Shaped Window Self-Attention
### 3.2.a. Horizontal and Vertical Stripes
### 3.2.b. Computation Complexity
### 3.2.c. Locally-Enhanced Positional Encoding
## 3.3. CSwin Transformer Block
## 3.4. Architecture Variants
