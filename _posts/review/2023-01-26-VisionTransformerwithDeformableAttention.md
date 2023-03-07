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
트랜스포머 모델은 크고,global한 수용장 덕에 CNN보다 Representation power가 좋지만 수용장이 크면 그만큼 메모리를 많이 차지하고 계산 비용이 많이 든다. 
또, 너무 넓게 보면 feature와 관련 없는 부분까지 영향을 받을 수도 있다. 이를 보완하기 위해 제안된 것이 PVT와 Swin인데 이들은 Sparse Attention을 하므로
계산량을 줄일 순 있지만 data agnostic하고 먼 거리에 있는 것과의 관계성은 잘 포착하지 못한다. 
이런 문제를 해결하기 위해 **key-value를 data-dependent하게 선택해야 한다. 그렇게 하면, 관련된 영역에 더욱 초점을 잘 맞출 수 있고 더 많은 정보를
포함한 feature를 포착할 수 있다.**

# 1. Introduction
기존의 Transformer는 데이터와 무관한 어텐션을 수행하므로 최적은 아닐 수 있다. 필요한 key/value는 삭제되고, 필요 없는 것들만 남을 수도 있기 때문이다. 
이를 극복하려면 **주어진 쿼리에 대한 후보 key/value쌍이 각각의 개별 input에 유연하게 대응할 수 있는 능력을 가지면 된다.** 
CNN논문에서도 컨볼루션 필터에 대해 **Deformable receptive field**을 학습하여, data-dependent 기반으로 정보가 더욱 풍부한 지역에 선택적으로 주의를 
기울일 수 있음을 보였다. 이 아이디어를 Vision Transformer에 그대로 구현하기엔 높은 메모리/계산 비용이 발생하므로 본 논문에서는 간단하고 효율적인
**Deformable self-attention**모듈을 제안한다. 기존의 DCN(Deformable Convolution Network)이 전체 피처맵의 픽셀마다 서로 다른 오프셋을 
학습했다면, **DAT(Deformable Attention Transformer)** 는 모든 쿼리들이 공유하는 샘플링 오프셋 그룹을 학습합니다. 그렇게 하면 key/value를 중요한 
영역으로 이동시킬 수 있다.      
![image](https://user-images.githubusercontent.com/69246778/214780171-e5ec7f19-5625-4f8c-a679-46aa14fd111d.png)   
(a) **ViT** : 모든 쿼리에 대해 global 어텐션을 합니다.   
(b) **Swin Transformer** : 윈도우 내에서만 어텐션을 합니다.   
(c) **DCN** : 각 쿼리에 대해, 서로 다른 형태의 deformed point을 학습합니다.   
(d) **DAT** : 모든 쿼리에 대해, 공유되는 deformed point를 학습합니다.   
   
각 어텐션 모듈의 reference point는 입력 데이터 전체에서 동일하게 균일 그리드로 먼저 생성된다. 그리고나서, 오프셋 네트워크가 모든 쿼리 feature를 
input으로 받아 reference point에 대한 오프셋을 생성합니다. 이러한 방식으로 key/value후보군들이 더 중요한 영역으로 이동해 보다 많은 정보를 캡처할 수 
있게 된다.
   
# 3. Deformable Attention Transformer
## 3.1. Preliminaries
ViT는 flatten된 피처맵을 인풋으로 받아서 여러 개의 head에 대해 Multi-head self-attention 을 진행한다.   
![image](https://user-images.githubusercontent.com/69246778/214783660-738fe646-e934-4cd7-9164-6bb2cead236d.png)   
q,k,v에 가중치를 곱해준 후 어텐션 연산을 수행하는데 이때, 각 헤드의 차원은 전체채널(C)을 head갯수(M)로 나눈 것으로, 그 수만큼 어텐션을 진행한다. 
각 헤드에서의 연산 결과를 concatenation한다.   
트랜스포머 블럭은 MLP와 GELU활성화함수로 구성되어 있고 normalization layer와 identity shortcut을 사용합니다.   
![image](https://user-images.githubusercontent.com/69246778/214784770-9ec82013-cea2-476b-95df-16430f0dcf4a.png)   

## 3.2. Deformable Attention
 PVT는 다운샘플링을 하기 때문에 정보가 손실될 수 있고, Swin은 수용장의 크기 성장이 제한되기에 큰 물체에 대한 모델링은 어렵다. 때문에, 
**data-dependent sparse attention**(데이터 기반으로 일부만 어텐션하는 것)이 필요하다. **서로 다른 쿼리끼리 유사한 어텐션 피처맵을 가지고 있다는 것을 활용하여
각 쿼리에 대해 key와 value의 이동을 공유하는 방식을 선택할 수 있다.**   
구체적으로, 본 논문에서는 **피처맵의 중요한 부분에서 토큰 간의 관계를 더 잘 보기 위한 deformable attention**을 제안한다. 초점 영역은 **오프셋 네트워크**가 
쿼리를 통해 학습한 deformed smpling points로 결정되며 **이중 선형 보간법**을 사용해서 피처맵으로부터 피처를 추출하고 추출된 피처를 key와 value에 적용해 
**deformed key와 deformed value**를 얻는다. 마지막으로, **멀티헤드어텐션**을 이용해 샘플링된 key에 대한 쿼리에 attend하고 deformed value로부터 피처를 
집계한다. 추가적으로, deformed point의 위치는 강력한 **relative position bias**를 제공해서 deformale attention을 더 용이하게 해준다.   

### 3.2.1 Deformable attention module
![image](https://user-images.githubusercontent.com/69246778/215389705-95dc5df4-10ef-4c2e-b85a-dfd1aa89181d.png)
'H x W x C'의 인풋 피처맵이 주어지면, HG x WG x 2'포인트들의 균일한 그리드가 레퍼런스로 생성된다. 그리드 크기는 입력 피처맵에서 H/r, W/r 만큼 다운샘플링된다.
레퍼런스 포인트의 값들은 선형적인 간격의 {(0,0},...,(HG-1,WG-1)}를 따르고 이를 [-1,+1]사이로 정규화한다. 
왼쪽 위를(-1,-1)로 나타내고 오른쪽 아래는(+1,+1)을 나타낸다. **쿼리 피처맵을 선형으로 투영하여 쿼리 토큰을 얻은 다음에 이를 가벼운 서브네트워크 θoffset(·)
에 보내면 각각의 레퍼런스 포인트에 대한 오프셋 ∆p = θoffset(q)을 얻을 수 있다.** 학습 과정을 안정화하기 위해서는 오프셋이 너무 커지면 안되기 때문에 
∆p ←− s tanh (∆p) 같은 방식으로 스케일을 조정한다. 그리고나서, **deformed points의 위치에서 선택된 피처들이 key와 values값이 된다.**



## 3.3. Model Architectures
![image](https://user-images.githubusercontent.com/69246778/215432933-0ca9277f-afc3-4793-a5c4-240c1285c57c.png)

Transformer에서 기본 MHSA(Multi Head Self Attention)를 Deformable Attention으로 바꾼다. 그리고 MLP와 결합해서 Deformable vision transformer block를 만든다.
네트워크 관점에서 볼 때, **Deformable Attention Transformer**는 피라미드 구조와 유사한 구조를 가져 다양한 스케일의 피처맵에 대해 광범위하게 적용할 수 있다. 
위의 그림처럼, H x W x 3크기의 이미지를 4x4 컨볼루션을 겹치지 않게 계산해서 H/4 x W/4 x C 크기로 임베딩하고 계층적인 피라미드 구조를 목표로, 
스테이지마다 점진적으로 stride를 증가시킨다.   
3,4번째 스테이지에서 연속적인 local attention과 deformable attention을 제안한다. 피처맵은 먼저 윈도우 베이스의 local attention을 통해 지역적인 정보를 
수집하고, deformable attention으론 지역적으로 퍼져있는 토큰들 간의 전체적인 관계를 포착한다. local 및 global한 수용장을 모두 사용해서 모델은 더 강한
representation power를 갖게 된다. 이는 [GLiT](https://arxiv.org/pdf/2107.02960.pdf), [TNT](Transformer in Transformer), [Point-former](https://openaccess.thecvf.com/content/CVPR2021/papers/Pan_3D_Object_Detection_With_Pointformer_CVPR_2021_paper.pdf)와 비슷하다.
처음 두 개의 스테이지는 주로 local한 특징을 학습하므로 deformable은 잘 사용하지 않고 게다가, 처음 두개의 스테이지의 key와 value는 보다 큰 spatial size를
가지므로 오버헤드가 발생해 deformable attention의 이중 선형보간을 크게 증가시킨다. 따라서, 초반에는 deformable attention을 사용하지 않고 마지막 2개에서만
deformable attention을 사용한다. 초반 단계에서는 Swin Transformer의 Shift-window attention을 사용한다. 
# 4. Experiments
![image](https://user-images.githubusercontent.com/69246778/223355196-026f3eb7-605e-4ff4-b395-5fddbaa42544.png)
![image](https://user-images.githubusercontent.com/69246778/223355227-5d96401f-8c1f-4601-a188-069e4c9b6021.png)
![image](https://user-images.githubusercontent.com/69246778/223355283-2baedf35-ee16-401d-b204-4135c9e35981.png)


## 논문요약
기존의 transformer 모델에서는 입력 패치들 간의 상호작용을 고정된 형태의 어텐션으로 처리하는데, 이 논문에서는 deformable attention 메커니즘을 도입하여 
입력 패치들 간의 상호작용을 보다 유연하게 처리한다. 제안된 모델은 이미지넷(ImageNet) 데이터셋에서 다른 모델들과 비교하여 우수한 성능을 보여주었으며, 
deformable attention이 모델의 성능 향상에 기여하는 것을 실험적으로 검증하였다.   
   
Deformable Attention은 입력 패치들 간의 상호작용을 처리하는 어텐션 메커니즘 중 하나로 기존의 어텐션이 입력 패치들 간의 상대적인 위치를 고정된 형태로 가정하는 것과
달리 **Deformable Attention은 입력 패치들 간의 상대적인 위치를 동적으로 조절하여 상호작용을 더 잘 처리할 수 있도록 한다.** Deformable Attention은 입력 
패치들의 위치를 조절하기 위해 **위치 이동 벡터(offset vector)** 를 학습한다. 이 위치 이동 벡터는 각 패치의 특성 정보에 따라서 동적으로 결정되며, 
이를 통해 입력 패치들 간의 상대적인 위치를 조절할 수 있다. 조정된 포인트에서 k,v를 선택한다.




