---
layout: post
title: 
description: |
  책 자연어 처리를 위한 트랜스포머 Review
hide_image: true
tags:
  - deeplearning
published: true
---

# 자연어 처리를 위한 트랜스포머, Denis Rothman - Chapter 01
* * *

# 1. 트랜스포머 모델 아키텍처

## 1.1. 트랜스포머의 배경
* **Markov Decision Processes(MDPs), Markov Chains, Markov Processses** : 시퀀스의 마지막 요소만을 사용해 사슬의 다음 요소를 예측한다.
* **RNNs** : 시퀀스의 지속적 상태(persisent states)를 효율적으로 기억하지만 고정된 크기의 벡터를 사용하므로 정보 손실과 기울기 소멸 문제 발생한다.
때문에 긴 문장에 대해선 번역을 잘 못한다. 
* **LSTM** : RNN의 진화 버전
* **CNN** : 레이어 별로 정보를 수집하는 지속상태에 기반한다.  매우 길고 복잡한 시퀀스에서 장기 종속성 처리시에 문제가 발생할 수 있다.
* **Attention** : 단순히 시퀀스의 마지막 토큰을 보지 않고 다른 토큰들도 들여다보는 방식이다. **인코더의 마지막 hidden layer뿐만 아니라 모든 
hidden layer를 디코더에 전달**한다. 디코더 단에서 출력단어를 예측하는 매 시점마다 인코더의 **전체**입력 문장을 다시 참고한다.

## 1.2. 트랜스포머의 부상 : Attention Is All You Need
(책에선 이걸 **Original transformer**라고 칭한다.)
![image](https://user-images.githubusercontent.com/69246778/161892838-94e1e8eb-6893-45b7-b8bf-f8044d69b6cb.png)
아키텍처는 6개 레이어를 가진 인코더 스택과 6개의 레이어를 가진 디코더 스택으로 구성되어 있다. 그리고 각 레이어에 서브레이어가 포함되어 있다.
* 왼쪽그림 : (인코더 스택의 6-레이어 중 하나)input이 **Attention 서브레이어**와 **FFN(FeedForwardNetwork)** 를 거쳐 인코더로 들어간다.
* 오른쪽 : (디코더 스택의 6-레이어 중 하나) 타겟 출력이 **두 개의 Attention 서브레이어**와 **FFN**을 거쳐 디코더로 들어간다. 
   
💡recurrence는 두 단어 사이의 거리가 멀어질수록 연산량이 높아지는데 이를 attention이 대체한다.(RNN,LSTM,CNN 사용X)   
attention은 **단어 대 단어(word-to-word)** 연산으로, 분석대상 단어가 다른 모든 단어와 어떻게 관련되어 있는지 찾아낸다.
   
### 1.2.1 인코더 스택
인코더 스택은 6개의 layer로 구성되어 있고 각 레이어는 **Muti-Head Attetion 서브레이어**와 **FFN**을 포함한다.(이런 구조가 6개 있는거. 그치만
각 레이어가 갖는 정보는 다르다. 예를 들어, 임베딩 레이어는 가장 처음 레이어에만 있다.)   
각 layer는 마치 십자낱말퀴즈처럼, 이전 레이어의 학습결과를 기반으로 단어 간의 연관성을 찾는다. 
#### A.입력임베딩
학습된 임베딩을 사용해 입력 토큰을 512차원 벡터로 변환하고자 한다. (original에서는 d_model상수를 512로 설정한다. 고정적인 상수값임
목표에 따라 다르게 설정할 수 있다.)   
   
1. **토크나이저(tokenizer)**가 문장을 토큰들로 변환한다.
```
the transformer is an innovation NLP model!
--> ['the', 'transform', 'er', 'is','an','innovative','n','l','p',model','!']
```
   
또한 토크나이저는 각 토큰에 대해 정수 표현을 제공한다. 
```
Text = "The cat slept on the couch. It was too tired to get up."
tokenized text = [1996, 4937, 7771, 2006, 1996, 6411, 1012, 2009, 2001, 2205, 5458, 2000, 2131, 2039, 1012]
```
그러나 위와 같은 **토큰화된 텍스트**는 정보가 불충분하다. 따라서, 이 토큰화된 텍스트를 **임베딩**해야한다.
   
2. **임베딩**   
**word2vec**방식의 **skip-gram**아키텍처를 선택한다. 이를 통해, **중심 단어에 초점**을 맞추고 중심단어 기준으로 문맥을 구성하는 
**다른 단어들을 분석**한다. 그런 다음, **창을 슬라이딩**시켜 위의 프로세스를 반복한다. 
   
3. **예시**
```
The black cat sat on the couch and the brown dog slept on the rug
```
위의 문장에서 **"black"** 과 **"brown"** 에 초점을 맞춘다면, 두 단어를 다음과 같이 512차원으로 표시할 수 있다. 
![image](https://user-images.githubusercontent.com/69246778/161905747-1585e77f-4940-4f41-b03e-c65541d988f9.png)
![image](https://user-images.githubusercontent.com/69246778/161905763-3e947c9e-d830-49e0-8fef-7d07e166a9db.png)
위의 두 단어 간의 유사도를 계산한다. **코싸인 유사도**를 이용하면

```
cosine_similarity(black,brown) = [[0.9998901]]
```
결과가 나온다. 두 단어가 유사하단 걸 확인할 수 있다. 
   
4. **결과**
위의 두 단어가 관련도가 높단 걸 파악했다. 따라서, 후속적으로 거치게 될 레이어는 **단어들이 어떻게 연관될 수 있는지**에 대한 정보를 
가진 채로 시작할 수 있다. 그러나, 여전히 단어의 위치에 대한 정보는 없으므로 많은 정보가 누락된다. 따라서 **위치 인코딩**이 필요하다.

#### B.위치인코딩
단어의 위치는 모르는 채로 위치 인코딩에 진입한다. 훈련 속도,비용,복잡성 등의 이유로 위치에 대한 정보를 서술하기 위해 새로운 벡터를 만드는게 
아니라 **기존 입력 임베딩에 위치 인코딩 값을 추가**한다. 






