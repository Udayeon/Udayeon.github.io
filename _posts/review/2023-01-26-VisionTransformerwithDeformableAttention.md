---
layout: post
title: 
description: |
  Vision Transformer with Deformable Attention, Zhuofan Xia, Xuran Pan, Shiji Song, Li Erran Li, 
  Gao Huang, Department of Automation, BNRist, Tsinghua University, 
  AWS AI, Amazon Beijing Academy of Artificial Intelligence
hide_image: true
tags:
  - review
published: true
---

# Vision Transformer with Deformable Attention
[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Xia_Vision_Transformer_With_Deformable_Attention_CVPR_2022_paper.html?ref=https://githubhelp.com)

* * *
**Abstract**   
트랜스포머 모델은 크고,global한 수용장 덕에 CNN보다 Representation power가 좋습니다. 그치만, 수용장이 크면 그만큼 메모리를 많이
차지하고 계산 비용이 많이 듭니다. 또, 너무 넓게 보면 feature와 관련 없는 부분까지 영향을 받을 수도 있습니다. 이를 보완하기 위해
제안된 것이 PVT와 Swin인데 이들은 Sparse Attention을 하므로 계산량을 줄일 순 있지만 data agnostic하고 먼 거리에 있는 것과의 
관계성은 잘 포착하지 못합니다. 이런 문제를 해결하기 위해 **key-value를 data-dependent하게 선택해야 합니다. 
그렇게 하면, 관련된 영역에 더욱 초점을 잘 맞출 수 있고 더 많은 정보를 포함한 feature를 포착할 수 있게됩니다.**

# 1. Introduction
 CNN기반 모델과 비교하면 트랜스포머 모델은 더 큰 수용장을 통해 거리가 먼 것과의 관계성도 잘 포착할 수 있고 많은 양의 훈련 데이터
및 모델 파라미터 영역에서 우수한 성능을 달성합니다. 그러나 과도한 attention은 **높은 계산 비용이 발생하고 수렴하는 속도가 
느리며 오버피팅**이 발생할 위험성이 높아집니다.   
   
과도한 어텐션 계산을 피하기 위해 Swin과 PVT라는 모델이 제안되었습니다. Swin은 window라고 하는 특정 영역 내에서만 어텐션을 진행하고 
PVT는 key,value의 피처맵을 다운샘플링해서 계산량을 줄입니다. 그러나, **이들 hand-craft방식은 효과적이지만 데이터와 무관하므로 
최적은 아닐 수 있습니다. 필요한 key/value는 삭제되고, 필요 없는 것들만 남을 수도 있습니다.**   
   
 이상적으로는, 주어진 쿼리에 대한 후보 key/value쌍이 각각의 개별 input에 유연하게 대응할 수 있는 능력을 갖는 것입니다. 
CNN논문에서도 컨볼루션 필터에 대해 **Deformable receptive field**을 학습하면, 
data-dependent기반으로 정보가 더욱 풍부한 지역에 선택적으로 주의를 기울일 수 있다고 합니다. 
이 아이디어를 Vision Transformer에 그대로 구현하기엔 높은 메모리/계산 비용이 발생합니다.   
   
본 논문에서는 간단하고 효율적인 **Deformable self-attention**모듈을 제안합니다. 다양한 비전 작업을 위해 **Deformable Attention
Transformer**백본을 설계했습니다. 기존의 DCN(Deformable Convolution Network)은 전체 피처맵의 픽셀마다 서로 다른 오프셋을 
학습했다면, **DAT(Deformable Attention Transformer)** 는 모든 쿼리들이 공유하는 몇 가지 샘플링 오프셋 그룹을 학습합니다. 
그렇게 하면 key/value를 중요한 영역으로 이동시킬 수 있습니다. 아래 사진을 참조하면,   
![image](https://user-images.githubusercontent.com/69246778/214780171-e5ec7f19-5625-4f8c-a679-46aa14fd111d.png)   
(a) **ViT** : 모든 쿼리에 대해 global 어텐션을 합니다.   
(b) **Swin Transformer** : 윈도우 내에서만 어텐션을 합니다.   
(c) **DCN** : 각 쿼리에 대해, 서로 다른 형태의 deformed point을 학습합니다.   
(d) **DAT** : 모든 쿼리에 대해, 공유되는 deformed point를 학습합니다.   
   
구체적으로 설명하면, 각 어텐션 모듈의 reference point는 입력 데이터 전체에서 동일하게 균일 그리드로 먼저 생성됩니다. 
그리고나서, 오프셋 네트워크가 모든 쿼리 feature를 input으로 받아 reference point
에 대한 오프셋을 생성합니다. 이러한 방식으로 key/value후보군들이 더 중요한 영역으로 이동해 보다 많은 정보를 캡처할 수 있습니다.
   
# 2. Related Work
## 2.1. Transformer vision backbone
dense prediction과 효율적인 어텐션 매커니즘을 위해 window attention, global tokens, focal attention, dynamic token sizes 등이 등장했습니다.
최근에는 컨볼루션이 Vision Transformer에 도입되었습니다. CVT(Convolutions to Vision Transformers)는 컨볼루션을 사용해 토큰화를 진행하고 계산량을 줄이기 위해
Stride 컨볼루션을 사용합니다. 또한, 컨볼루션이 Vision Transformer초기 단계에 추가되는 것이 더 안정적이라고 제안하는 [논문](https://arxiv.org/pdf/2106.14881.pdf)도 있습니다.   
CSwin Transformer는 컨볼루션 기반의 위치 인코딩 기술을 채택하기도 했습니다. 이버한 컨볼루션 기반 기술들은 DAT에 적용 가능합니다.

## 2.2. Deformable CNN and attention 
Deformable Convoluiton은 입력데이터에 따라 조정된 유연한 spatial location에 주의를 기울이는 방법입니다. 최근에는 Vision Transformer에 적용되었습니다.
deformable DETR

# 3. Deformable Attention Transformer
## 3.1. Preliminaries
최근 ViT의 어텐션 메커니즘을 다시 한번 살펴봅시다. ViT는 flatten된 피처맵을 인풋으로 받아서 여러 개의 head에 대해 
Multi-head self-attention 을 진행합니다.   
![image](https://user-images.githubusercontent.com/69246778/214783660-738fe646-e934-4cd7-9164-6bb2cead236d.png)   
q,k,v에 가중치를 곱해준 후 어텐션 연산을 수행합니다. 이때, 각 헤드의 차원은 전체채널(C)을 head갯수(M)로 나눈 것으로, 그 수만큼
어텐션을 진행합니다. 각 헤드에서의 연산 결과를 concatenation합니다.   
트랜스포머 블럭은 MLP와 GELU활성화함수로 구성되어 있고 normalization layer와 identity shortcut을 사용합니다.   
![image](https://user-images.githubusercontent.com/69246778/214784770-9ec82013-cea2-476b-95df-16430f0dcf4a.png)   

## 3.2. Deformable Attention
 PVT는 다운샘플링을 하기 때문에 정보가 손실될 수 있고, Swin은 수용장의 크기 성장이 제한되기에 큰 물체에 대한 모델링은 어렵습니다.
때문에, **data-dependent sparse attention**(데이터 기반으로 일부만 어텐션하는 것)이 필요합니다. 
**서로 다른 쿼리끼리 유사한 어텐션 피처맵을 가지고 있다는 것을 활용하여 각 쿼리에 대해 key와 value의 이동을 공유하는 
방식을 선택합니다.**   
구체적으로, 우리는 피처맵의 중요한 부분에서 토큰 간의 관계를 더 잘 보기 위한 deformable attention을 제안합니다.
초점 영역은 오프셋 네트워크가 쿼리를 통해 학습한 deformed smpling points로 결정됩니다. 우리는 이중 선형 보간법을 사용해서
피처맵으로부터 피처를 추출하고 추출된 피처를 key와 value에 적용하여 deformed key와 deformed value를 얻습니다. 
마지막으로, 멀티헤드어텐션을 이용해 샘플링된 key에 대한 쿼리에 attend하고 deformed value로부터 피처를 집계합니다.
추가적으로, deformed point의 위치는 강력한 relative position bias를 제공해서 deformale attention을 더 용이하게 해줍니다.

### 3.2.1 Deformable attention module
### 3.2.2 Offset generation
### 3.2.3. Offset groups
### 3.2.4. Deformable relative position bias
### 3.2.5. Computational complexity

## 3.3. Model Architectures

# 4. Experiments
## 4.1. ImageNet-1k Classification
## 4.2. COCO Object Detection
## 4.3. ADE20K Semantic Segmentation
## 4.4. Ablation Study






