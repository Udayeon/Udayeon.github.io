---
layout: post
title: About AI
description: |
  ANN(Artificial Neural Network)와 Deep learning
hide_image: true
tags:
  - ai
published: true
---

# 1. ANN과 Deep Learning
* * *

- 1.1. ANN(Artificial Neural Network)
- 1.2. Deep Learning의 비약적 발전
  - 1.2.1. Algorithm의 발전
    - 1.2.1.A. 비지도학습을 이용한 전처리
    - 1.2.1.B. Convolutional Neural Network(합성곱 신경망)
    - 1.2.1.C. Rectified Linear Unit
    - 1.2.1.D. Drop-out
  - 1.2.2. Hardware의 발전
    - 1.2.2.A. 범용 GPU
  - 1.2.3. 빅데이터의 등장
- 1.3. ANN의 구조
  - 1.3.1. 퍼셉트론
    - 1.3.1.A. 퍼셉트론의 기능 - 논리gate 구현
    - 1.3.1.B. 퍼셉트론의 한계 - Exclusive OR (XOR)
    - 1.3.1.C. 퍼셉트론의 한계 극복 - 2개의 퍼셉트론으로 XOR 해결
  - 1.3.2. 활성화함수
    - 1.3.2.A. Sigmoid
    - 1.3.2.B. Hyperbolic tangent
    - 1.3.2.C. ReLU(Rectified Linear Unit
- 1.4. 신경망에서의 신호전달
## 1.1. ANN(Artificial Neural Network)
인간의 신경망(뇌)에서 영감을 얻은 통계학적 학습 알고리즘. 시냅스의 결합으로 네트워크를 형성한 인공 뉴런(node)이 학습을 통해
시냅스의 결합 세기를 변화시켜 문제 해결 능력을 가지는 모델 전반을 말함.   
![image](https://user-images.githubusercontent.com/69246778/128827637-e74578b6-b35f-458a-81cb-6e5ebe487520.png)   
Deep Learning은 인공신경망이 deep해지면서 레이어가 많아진 것. 인공지능을 이루기 위한 Maching Learning이라는 방법론 중 하나가 딥러닝.
   
## 1.2. Deep Learning의 비약적 발전

### 1.2.1. Algorithm의 발전
over-fitting과 Vanishing Gradient(기울기 소실)을 해결. 

#### 1.2.1.A. 비지도학습을 이용한 전처리
가중치 감쇠 규제
#### 1.2.1.B. Convolutional Neural Network(합성곱 신경망)
#### 1.2.1.C. Rectified Linear Unit
새로운 활성화함수 ReLU의 도입
#### 1.2.1.D. Drop-out
어떤 신경망이 over-fitting을 피하기 위해 학습과정의 결과를 일부러 버리는 아니러니컬한 idea.

### 1.2.2. Hardware의 발전
#### 1.2.2.A. 범용 GPU
병렬적인 연산 수행에 사용. 학습시간 대폭 단축

### 1.2.3. 빅데이터의 등장
ANN은 Weight의 수가 많아 이를 모두 학습시키기에는 많은 양의 data가 필요. Big data의 등장으로 인공신경망이 deep할 때도 충분히 학습시킬 수
있어서 성능 향상됨.

## 1.3. ANN의 구조
![image](https://user-images.githubusercontent.com/69246778/128829602-13a7ba4a-02cc-4c98-99b4-b0c2f6d41f2c.png)

### 1.3.1. 퍼셉트론
ANN을 이루는 기본 구조.
![image](https://user-images.githubusercontent.com/69246778/128829953-054dd8e4-ac24-4fb0-b9e7-bc2b8bd5f5a0.png)   
x1부터 xd까지 d차원의 input을 받을 때, 이 각각의 d차원 input에 곱해지는 가중치가 w1부터 wd까지 존재함.
이 w와 x를 곱한 것들의 합이(즉, 벡터w와 벡터x의 내적) Threshold value보다 크면 1, 작으면 0으로 y값을 결정.
이렇게 결정해주는 함수를 활성화함수 h라 함. h는 step function임.
![image](https://user-images.githubusercontent.com/69246778/128831273-22eea691-0cd3-4721-a956-e2146604c578.png)

#### 1.3.1.A. 퍼셉트론의 기능 - 논리gate 구현
* AND gate   
![image](https://user-images.githubusercontent.com/69246778/128832716-d0ce3485-9907-4f32-b533-0827709b0cd1.png)   
input(x1,x2) 2개가 주어지는 경우 1,1이 들어오는 경우에만 1이 나오는 operator. weight값을 어떻게 설정하느냐로 결정됨.   
   
* OR gate   
![image](https://user-images.githubusercontent.com/69246778/128832679-44c939c6-e670-4ee9-8ffb-21dbb2ff6ed7.png)   
input 2개가 주어지는 경우 하나라도 1이면 1이 나오는 operator.   

이 퍼셉트론이 다층 퍼셉트론으로 그리고 딥러닝으로 이어졌음

#### 1.3.1.B. 퍼셉트론의 한계 - Exclusive OR (XOR)
같은 input이 들어오는 경우에만 1이 나오는 operator.
![image](https://user-images.githubusercontent.com/69246778/128833771-eaf4d5cb-de44-42bd-b894-970006320144.png)
퍼셉트론은 선형분류기(Threshold보다 크냐 작냐로 분류)인데 이런 경우는 선형 분리가 안됨. 대신 퍼셉트론 층을 쌓으면 가능

#### 1.3.1.C. 퍼셉트론의 한계 극복 - 2개의 퍼셉트론으로 XOR 해결
![image](https://user-images.githubusercontent.com/69246778/128834062-1d1e7529-7f2d-4a77-a757-07e62dd0ed05.png)
1. 첫 번째 퍼셉트론 : NAND(Not AND, 선형분류기)   
2. 두 번째 퍼셉트론 : OR(선형분류기)   
1,2를 첫 번째 Hidden layer로, AND를 두 번째 Hidden layer로 삼으면 1과2의 결과를 AND처리한 것.   
1,2 모두 +1이면 검정색으로 분류, 그렇지 않으면 흰색으로 분류.   
즉, 공간변환이 일어난 평면상에서 선형분리를 통해 XOR function을 구현하는 것.

### 1.3.2. 활성화함수
퍼셉트론은 Step function을 활성화함수로 사용.
![image](https://user-images.githubusercontent.com/69246778/128834985-f7de7130-5186-4698-8714-3f6d98fc9f3f.png)
0을 중심으로 -1과 1을 구분하는 function. 신경망 학습에는 미분값이 필요한데, 이 함수의 미분값은 대부분 0이 되고 심지어 0에서는 미분값을
구할수도 없음. 좋은 함수는 아님. 그래서 이 함수를 좀 부드러운 function으로 전환해줄 필요가 있음. 다음에 나오는 활성화 함수들이
step function을 대체할 수 있는 함수들임.

#### 1.3.2.A. Sigmoid
![image](https://user-images.githubusercontent.com/69246778/128835541-5ec23809-37e2-4630-9cd6-80c53bd283ed.png)

#### 1.3.2.B. Hyperbolic tangent
![image](https://user-images.githubusercontent.com/69246778/128835563-eb17a31a-25b8-419c-a1ac-50705f2e36b8.png)

#### 1.3.2.C. ReLU(Rectified Linear Unit
![image](https://user-images.githubusercontent.com/69246778/128835588-6f728a25-a592-49ee-8278-2b47f8b31182.png)

## 1.4. 신경망에서의 신호전달
![image](https://user-images.githubusercontent.com/69246778/128836197-934ffcae-35dc-4ff8-8715-092e01b98cf2.png)   
   
* 입력층 -> 1층
![image](https://user-images.githubusercontent.com/69246778/128836311-66949873-e09f-4e2f-a501-5163eac1a8e3.png)
input x1에 가중치 1이 곱해지고, x2에는 가중치 2가 곱해져서 서로 더함. 이 더해진 결과를 a라 할때 각 결과 들이 hidden layer의
노드에 각각 전달. 이 때, weight의 집합을 W(1) 매트릭스로 표현할 수 있는데, Threshold가 되는 bias도 매트릭스에 넣어서 
한번에 생각할 수 있음.    
   
![image](https://user-images.githubusercontent.com/69246778/128836337-7d7faa31-526c-40e6-bd6d-1ea586e2a24b.png)
즉, a는 x와 W(1)의 곱으로 표현됨.   
   
![image](https://user-images.githubusercontent.com/69246778/128959070-689f7906-dad1-4c71-b998-201548b55e5a.png)
위의 결과를 활성화함수 h에 통과시키면 첫번째 hidden layer에 최종적인 결과 z가 구해짐. 이때, 활성화함수는 가장 많이 사용되는 sigmoid가 
될 수도 있고 다른 함수가 될 수도 있음. Deep Learning으로 deep해지면 ReLU함수를 많이 사용함.

* 1층 -> 2층
전과 비슷한 과정을 거침. weight vector를 설정하고 그 weight를 z1의 결과와 곱하면 hidden layer 2층의 결과가 됨. 
![image](https://user-images.githubusercontent.com/69246778/128959714-dc0c3b94-890e-40ae-8929-2bd7868ee924.png)
   
이런 과정을 거쳐 3층까지 가게되면 결국 출력층에 도달하는데, 출력층에서 주로 쓰는 함수는 **Softmax**함수임
![image](https://user-images.githubusercontent.com/69246778/128959869-3661ef38-863e-436b-be1f-d816c7327f9b.png)

### 1.5. one-hot encoding
출력층의 자리를 가지고 분류하고자 하는 카테고리를 나타내는 방식

