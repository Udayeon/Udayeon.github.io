---
layout: post
title: 
description: |
  (arXiv)3 May 2022, Lucas Beyer Xiaohua Zhai Alexander Kolesnikov, Google Research, Brain Team Zurich
hide_image: true
tags:
  - review
published: true
---

# Better plain ViT baselines for ImageNet-1k
[링크](https://arxiv.org/pdf/2205.01580v1.pdf)
* * *
기존 ViT의 기본적인 설정을 사소하게 수정하는 것만으로도 성능 개선이 가능함을 제시한다.

# 1. Introduction
기존 ViT모델은 대규모 사전 훈련에만 초점을 맞췄다. 본 논문에서는 기존 ViT의 심플함에 충실하면서도 비슷한 접근법을 이용해 결과를 
얻을 수 있는 방법을 제안한다.

# 2. Experimental setup
(사전)훈련과 평가를 위해 ImageNet-1k dataset (ILSVRC2012)에 전적으로 초점을 맞췄으며 기존 ViT의 광범위한 수용성, 단순성,
 확장성을 고수하고 아주 사소한 부분만 재점검할 뿐 새로운 것은 없다. 
![image](https://user-images.githubusercontent.com/69246778/167350334-3ab7bbd7-3fce-4b79-8708-6526ed18803f.png)
![image](https://user-images.githubusercontent.com/69246778/167350401-5c3b6c95-0362-46f0-9888-9935f57a24f1.png)


# 3. Results
개선된 설정에 대한 결과는 그림1에서 확인할 수 있다.
![image](https://user-images.githubusercontent.com/69246778/167350589-6065ff91-4d09-499f-8974-e05ee0a163a7.png)
이런 방식으로 훈련된 ViT는 80%의 성능을 내는 300 epoch까지 21시간 40분 걸림. 글고 성능도 좋아짐.
   
수정한 부분은 다음과 같음
- Random Augmentation, Mixup 사용
- Position Embedding : fixed 2D sin-cos
- batch_size : 4096 -> 1024
- class token -> GAP(Global Average Pooling)
- Head : linear -> MLP
![image](https://user-images.githubusercontent.com/69246778/167357763-bdac77c3-cc17-4b8e-a454-20c8197967f0.png)
최종적으로 epoch 300일 때를 보면, Original보다 약13%성능이 증가한 걸 볼 수 있다. 수정된 요소가 각각 어떤 영향을
미치는지 살펴보면 Head의 변경이 가장 작은 영향을 미치고 Random Augmentation+MixUp이 가장 큰 영향을 미친다.

# 4. Conclusion
![image](https://user-images.githubusercontent.com/69246778/167358667-f4aab3bb-25a0-4101-8418-f88b2f4f693d.png)
단순한 것을 추구하는 것은 항상 가치 있다.
