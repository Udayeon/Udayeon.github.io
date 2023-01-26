---
layout: post
title: 
description: |
  Vision Transformer with Deformable Attention, Zhuofan Xia, Xuran Pan, Shiji Song, Li Erran Li, 
  Gao Huang, Department of Automation, BNRist, Tsinghua University, 
  AWS AI, Amazon 3Beijing Academy of Artificial Intelligence
hide_image: true
tags:
  - review
published: true
---

# Vision Transformer with Deformable Attention
[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Xia_Vision_Transformer_With_Deformable_Attention_CVPR_2022_paper.html?ref=https://githubhelp.com)

* * *
**Abstract**
 트랜스포머는 크고 전역적인 수용장 덕에 CNN보다 더 높은 Represntation Power를 갖습니다. 
그치만, 단순히 수용장이 큰 것(global하게 모든 부분에 대해 계산)은 과도한 메모리 및 계산 비용을 발생시키고 feature는 관련 없는 부분의 영향까지 받게되는 문제가 발생합니다.
이런 문제를 해결하기 위해 Sparse하게 어텐션을 진행하는 PVT(Pyramid Vision Transformer)나 Swin Transformer가 등장했습니다. 
이들은 local한 계산(특정 영역 내에서만 계산)을 수행하므로 data agnostic(데이터와 무관한)하고 먼 거리에 있는 것과의 관계성 
포착은 제한될 겁니다. 이런 문제를 완화하기 위해 'Deformable self-attention'모듈을 제안합니다. 
'Deformable self-attention'은 셀프어텐션 계산 시 사용되는 **key-value쌍의 위치를 데이터에 기반하여 선택**합니다. 
때문에, 셀프어텐션 모듈이 **관련된 영역에 초점**을 맞출 수 있게 되고 **더 많은 정보를 포함한 feature를 포착**할 수 있습니다. 
Deformable self-attention을 사용한 **Deformable Attention Transformer**는 이미지 분류와 dense추정의 백본으로 사용할 수 있습니다.


# 1. Introduction
 자연어 처리에 사용되던 트랜스포머가 컴퓨터 비전 분야에서 높은 성능을 달성하고 있습니다. CNN기반 모델과 비교하면, 트랜스포머 모델은 
더 큰 수용장을 통해 거리가 먼 것과의 관계성도 잘 포착할 수 있습니다. 그치만 수용장이 크다고 마냥 좋은 것만은 아닙니다. 
**계산량이 증가**하고, **수렴하는 속도가 느리며**, **오버피팅**이 발생할 위험성이 높아집니다. 
과도한 어텐션 계산을 피하기 위해 제안된 대표적인 두 가지 모델은 'Swin Transformer'와 'PVT(Pyramid Vision Transformer)'입니다. 
Swin은 window라고 하는 특정 영역 내에서만 계산을 진행하고 PVT는 key,value의 피처맵을 다운샘플링해서 계산량을 줄입니다. 
그치만, **hand-craft방식은 효과적이지만 데이터와 무관하므로 최적은 아닐 수 있습니다. 
필요한 key/value는 삭제되고, 필요 없는 것만 남을 수도 있다는 거죠.**   
   
 이상적인건, 주어진 쿼리에 대한 key/value쌍이 각각의 input에 유연하게 대할 수 있는 능력을 갖는 것입니다. input에 따라 다르게 
어텐션을 진행하면 데이터의 정보를 더 잘 포착할 수 있을 겁니다. CNN논문에서도 컨볼루션 필터에 대해 **변형 가능한 수용장**을 
학습하면, 데이터에 기반하기 때문에 정보가 더욱 풍부한 지역을 선택적으로 참여할 수 있다고 합니다. 이 아이디어를 비전 트랜스포머에 
적용하면 좋겠지만 계산량이 너무 많아집니다.   
   
본 논문에서 제안하는 **Deformable Attention Transformer(DAT)** 는 **deformable self-attention module**을 피라미드처럼 
쌓은 것입니다. 기존의 **DCN(Deformable Convolution Network)** 은 전체 피처맵의 픽셀마다 다른 오프셋을 학습했다면,
**DAT**는 query agnostic(쿼리와 무관한)오프셋 일부 그룹만을 학습할 것을 제안합니다. 특정 쿼리에 구애받는 것이 아니라
모든 쿼리가 공유하는 영역에 대해 학습합니다. 이렇게하면, 중요한 영역으로 key와 value의 위치를 옮길 수 있습니다.   
   
![image](https://user-images.githubusercontent.com/69246778/214780171-e5ec7f19-5625-4f8c-a679-46aa14fd111d.png)   
(a) 모든 쿼리에 대해 global 어텐션을 합니다.   
(b) 각 쿼리를 포함한 윈도우 내에서만 어텐션을 합니다.   
(c) 각 쿼리에 대해 서로 다른 형태의 어텐션 영역을 지정합니다.   
(d) 모든 쿼리가 서로의 변형된 어텐션 영역을 공유합니다. (c는 각 쿼리가 각각 자신의 영역을 가짐)   
   
각 어텐션 모듈에서 reference points는 먼저 균일한 그리드에서, 전체 입력 이미지에 대해 발생합니다. 그리고나서, 쿼리 피처를
입력으로 받은 오프셋 네트워크가 모든 reference points에 대해 오프셋을 진행합니다. 그렇게하면 **key/value가 중요한 지역으로
이동하게 됩니다.** 



# 2. Related Work
## 2.1. Transformer vision backbone
## 2.2. Deformable CNN ans attention 

# 3. Deformable Attention Transformer
## 3.1. Preliminaries
최근 ViT의 어텐션 메커니즘을 다시 한번 살펴봅시다. ViT는 'N(파라미터 수) x C(채널)'크기의 인풋으 받아서 여러 개의 head에 대해
Multi-head self-attention 을 진행합니다.   
![image](https://user-images.githubusercontent.com/69246778/214783660-738fe646-e934-4cd7-9164-6bb2cead236d.png)   
q,k,v에 가중치를 곱해준 후 어텐션 연산을 수행합니다. 이때, 각 헤드의 차원은 전체채널(C)을 head갯수(M)로 나눈 것으로, 그 수만큼
어텐션을 진행합니다. 각 헤드에서의 연산 결과를 concatenation합니다.   
 트랜스포머 블럭은 MLP와 GELU활성화함수로 구성되어 있고 정규화화 identity shortcut을 사용합니다.
 ![image](https://user-images.githubusercontent.com/69246778/214784770-9ec82013-cea2-476b-95df-16430f0dcf4a.png)   
 
 ## 3.2. Deformable Attention
  PVT는 다운샘플링하므로 정보 손실을 초래하고, Swin은 수용장의 크기 성장이 제한되기에 큰 물체에 대한 모델링은 어렵습니다.
 때문에, **data-dependent sparse attention**이 필요한데, DCN에서 사용한 deformable한 모듈을 트랜스포머에 적용하면 계산량이 
 급증하게 됩니다. 따라서 **서로 다른 쿼리끼리 유사한 어텐션 피처맵을 가지고 있는 것을 활용하여 각 쿼리에 대해 key와 value의
 이동을 공유하는 방식을 선택합니다.**
 
 

