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

# ANN과 Deep Learning
* * *

- 1. ANN(Artificial Neural Network)
- 2. Deep Learning의 비약적 발전
  - 2.1. Algorithm의 발전
    - 2.1.A. 비지도학습을 이용한 전처리
    - 2.1.B. Convolutional Neural Network(합성곱 신경망)
    - 2.1.C. Rectified Linear Unit
    - 2.1.D. Drop-out
  - 2.2. Hardware의 발전
    - 2.2.A. 범용 GPU
  - 2.3. 빅데이터의 등장
- 3. ANN의 구조
  - 3.1. 퍼셉트론
    - 3.1.A. 퍼셉트론의 기능 - 논리gate 구현
    - 3.1.B. 퍼셉트론의 한계 - Exclusive OR (XOR)
    - 3.1.C. 퍼셉트론의 한계 극복 - 2개의 퍼셉트론으로 XOR 해결
  - 3.2. 활성화함수
    - 3.2.A. Sigmoid
    - 3.2.B. Hyperbolic tangent
    - 3.2.C. ReLU(Rectified Linear Unit
- 4. 신경망에서의 신호전달
  - 4.1. 입력층 -> 1층
  - 4.2. 1층 -> 2층
  - 4.3. 출력층 설계 - one-hot encoding
- 5. Batch processing (일괄처리)
- 6. 신경망 학습
  - 6.1. 학습 알고리즘
  - 6.2. Backpropagation(오차역전파법)
  - 6.3. over-fitting방지
    - 6.3.A. Early Stopping(조기종료)
    - 6.3.B. Regularization(규제)
    - 6.3.C. Drop-out 
    - 6.3.D. data augmentation(데이터 증식)
  - 6.4. Gradient vanishing과 Exploding방지
    - 6.4.A. Weight Initialization(가중치 초기화)
    - 6.4.B. 활성화 함수
    - 6.4.C. Batch Normalization(배치 정규화)
- 7. 딥러닝이 강력한 이유

# 1. ANN(Artificial Neural Network)
인간의 신경망(뇌)에서 영감을 얻은 통계학적 학습 알고리즘. 시냅스의 결합으로 네트워크를 형성한 인공 뉴런(node)이 학습을 통해
시냅스의 결합 세기를 변화시켜 문제 해결 능력을 가지는 모델 전반을 말함.   
![image](https://user-images.githubusercontent.com/69246778/128827637-e74578b6-b35f-458a-81cb-6e5ebe487520.png)   
Deep Learning은 인공신경망이 deep해지면서 레이어가 많아진 것. 인공지능을 이루기 위한 Maching Learning이라는 방법론 중 하나가 딥러닝.
   
# 2. Deep Learning의 비약적 발전

## 2.1. Algorithm의 발전
over-fitting과 Vanishing Gradient(기울기 소실)을 해결. 

### 2.1.A. 비지도학습을 이용한 전처리
가중치 감쇠 규제
### 2.1.B. Convolutional Neural Network(합성곱 신경망)
### 2.1.C. Rectified Linear Unit
새로운 활성화함수 ReLU의 도입
### 2.1.D. Drop-out
어떤 신경망이 over-fitting을 피하기 위해 학습과정의 결과를 일부러 버리는 아니러니컬한 idea.

## 2.2. Hardware의 발전
### 2.2.A. 범용 GPU
병렬적인 연산 수행에 사용. 학습시간 대폭 단축

## 2.3. 빅데이터의 등장
ANN은 Weight의 수가 많아 이를 모두 학습시키기에는 많은 양의 data가 필요. Big data의 등장으로 인공신경망이 deep할 때도 충분히 학습시킬 수
있어서 성능 향상됨.

# 3. ANN의 구조
![image](https://user-images.githubusercontent.com/69246778/128829602-13a7ba4a-02cc-4c98-99b4-b0c2f6d41f2c.png)

## 3.1. 퍼셉트론
ANN을 이루는 기본 구조.
![image](https://user-images.githubusercontent.com/69246778/128829953-054dd8e4-ac24-4fb0-b9e7-bc2b8bd5f5a0.png)   
x1부터 xd까지 d차원의 input을 받을 때, 이 각각의 d차원 input에 곱해지는 가중치가 w1부터 wd까지 존재함.
이 w와 x를 곱한 것들의 합이(즉, 벡터w와 벡터x의 내적) Threshold value보다 크면 1, 작으면 0으로 y값을 결정.
이렇게 결정해주는 함수를 활성화함수 h라 함. h는 step function임.
![image](https://user-images.githubusercontent.com/69246778/128831273-22eea691-0cd3-4721-a956-e2146604c578.png)

### 3.1.A. 퍼셉트론의 기능 - 논리gate 구현
* AND gate   
![image](https://user-images.githubusercontent.com/69246778/128832716-d0ce3485-9907-4f32-b533-0827709b0cd1.png)   
input(x1,x2) 2개가 주어지는 경우 1,1이 들어오는 경우에만 1이 나오는 operator. weight값을 어떻게 설정하느냐로 결정됨.   
   
* OR gate   
![image](https://user-images.githubusercontent.com/69246778/128832679-44c939c6-e670-4ee9-8ffb-21dbb2ff6ed7.png)   
input 2개가 주어지는 경우 하나라도 1이면 1이 나오는 operator.   

이 퍼셉트론이 다층 퍼셉트론으로 그리고 딥러닝으로 이어졌음

### 3.1.B. 퍼셉트론의 한계 - Exclusive OR (XOR)
같은 input이 들어오는 경우에만 1이 나오는 operator.
![image](https://user-images.githubusercontent.com/69246778/128833771-eaf4d5cb-de44-42bd-b894-970006320144.png)
퍼셉트론은 선형분류기(Threshold보다 크냐 작냐로 분류)인데 이런 경우는 선형 분리가 안됨. 대신 퍼셉트론 층을 쌓으면 가능

### 3.1.C. 퍼셉트론의 한계 극복 - 2개의 퍼셉트론으로 XOR 해결
![image](https://user-images.githubusercontent.com/69246778/128834062-1d1e7529-7f2d-4a77-a757-07e62dd0ed05.png)
1. 첫 번째 퍼셉트론 : NAND(Not AND, 선형분류기)   
2. 두 번째 퍼셉트론 : OR(선형분류기)   
1,2를 첫 번째 Hidden layer로, AND를 두 번째 Hidden layer로 삼으면 1과2의 결과를 AND처리한 것.   
1,2 모두 +1이면 검정색으로 분류, 그렇지 않으면 흰색으로 분류.   
즉, 공간변환이 일어난 평면상에서 선형분리를 통해 XOR function을 구현하는 것.

## 3.2. 활성화함수
퍼셉트론은 Step function을 활성화함수로 사용.
![image](https://user-images.githubusercontent.com/69246778/128834985-f7de7130-5186-4698-8714-3f6d98fc9f3f.png)
0을 중심으로 -1과 1을 구분하는 function. 신경망 학습에는 미분값이 필요한데, 이 함수의 미분값은 대부분 0이 되고 심지어 0에서는 미분값을
구할수도 없음. 좋은 함수는 아님. 그래서 이 함수를 좀 부드러운 function으로 전환해줄 필요가 있음. 다음에 나오는 활성화 함수들이
step function을 대체할 수 있는 함수들임.

### 3.2.A. Sigmoid
![image](https://user-images.githubusercontent.com/69246778/128835541-5ec23809-37e2-4630-9cd6-80c53bd283ed.png)

### 3.2.B. Hyperbolic tangent
![image](https://user-images.githubusercontent.com/69246778/128835563-eb17a31a-25b8-419c-a1ac-50705f2e36b8.png)

### 3.2.C. ReLU(Rectified Linear Unit
![image](https://user-images.githubusercontent.com/69246778/128835588-6f728a25-a592-49ee-8278-2b47f8b31182.png)

# 4. 신경망에서의 신호전달
![image](https://user-images.githubusercontent.com/69246778/128836197-934ffcae-35dc-4ff8-8715-092e01b98cf2.png)   

## 4.1. 입력층 -> 1층
![image](https://user-images.githubusercontent.com/69246778/128836311-66949873-e09f-4e2f-a501-5163eac1a8e3.png)
input x1에 가중치 1이 곱해지고, x2에는 가중치 2가 곱해져서 서로 더함. 이 더해진 결과를 a라 할때 각 결과 들이 hidden layer의
노드에 각각 전달. 이 때, weight의 집합을 W(1) 매트릭스로 표현할 수 있는데, Threshold가 되는 bias도 매트릭스에 넣어서 
한번에 생각할 수 있음.    
   
![image](https://user-images.githubusercontent.com/69246778/128836337-7d7faa31-526c-40e6-bd6d-1ea586e2a24b.png)
즉, a는 x와 W(1)의 곱으로 표현됨.   
   
![image](https://user-images.githubusercontent.com/69246778/128959070-689f7906-dad1-4c71-b998-201548b55e5a.png)
위의 결과를 활성화함수 h에 통과시키면 첫번째 hidden layer에 최종적인 결과 z가 구해짐. 이때, 활성화함수는 가장 많이 사용되는 sigmoid가 
될 수도 있고 다른 함수가 될 수도 있음. Deep Learning으로 deep해지면 ReLU함수를 많이 사용함.

## 4.2. 1층 -> 2층
전과 비슷한 과정을 거침. weight vector를 설정하고 그 weight를 z1의 결과와 곱하면 hidden layer 2층의 결과가 됨. 
![image](https://user-images.githubusercontent.com/69246778/128959714-dc0c3b94-890e-40ae-8929-2bd7868ee924.png)
   
이런 과정을 거쳐 3층까지 가게되면 결국 출력층에 도달하는데, 출력층에서 주로 쓰는 함수는 **Softmax**함수임
![image](https://user-images.githubusercontent.com/69246778/128959869-3661ef38-863e-436b-be1f-d816c7327f9b.png)

## 4.3. 출력층 설계 - one-hot encoding
출력층의 자리를 가지고 분류하고자 하는 카테고리를 나타내는 방식. 표현하고자 하는 단어의 인덱스 위치에 1을 부여하고, 다른 단어의 인덱스 
위치에는 0을 부여. 

# 5. Batch processing (일괄처리)
input 벡터 여러 개를 모아서 한꺼번에 학습하면 computing효율이 좋아짐. 

# 6. 신경망 학습
training data로 부터 weight매개변수의 최적값을 자동으로 획득하는 것. 이 때 사용하는 함수가 loss function. 이 loss function을 이용해
결과값을 목표치에 가깝게 만드는 방법이 [Gradient desecent method](https://udayeon.github.io/2021/07/06/about-ai/#211a-%EA%B2%BD%EC%82%AC%ED%95%98%EA%B0%95%EB%B2%95gradient-descent-method)

## 6.1. 학습 알고리즘
![image](https://user-images.githubusercontent.com/69246778/128960974-d20f471d-a412-419a-ae63-b84111eb5b8c.png)
* Mini batch : batch를 만들되 모든 training set으로 만드는게 아니라 그 중 일부를 선택해서 작은 set을 만들고 이를 한 번에 통과시키면서 
신경망을 훈련.
   
![image](https://user-images.githubusercontent.com/69246778/128960996-c1fbfe42-5323-4562-8d75-e5d78ff1ee9b.png)
학습과정이 진행됨에 따라 위와같이 손실함수가 감소함. 따라서 정확도는 증가함. 이렇게 되면 학습이 잘 되고 있는 것.

## 6.2. Backpropagation(오차역전파법)
학습을 위해서는 미분값이 필요. 이런 복잡한 미분은 보통 수치미분을 통해 수행하는데, 수치미분은 그 많은 weight를 일일히 계산해야 하므로
시간이 매우 오래 걸림. 따라서 등장한 방법이 back propagation.
x의 변화에 대해 z가 어느정도로 영향을 받는지를 나타낸 것이 미분임. 이를 다음과 같이 표기함.      
![image](https://user-images.githubusercontent.com/69246778/128969221-8b9e7c0c-b61b-4d51-b177-28aa42a910af.png)
   
![image](https://user-images.githubusercontent.com/69246778/128969133-0b35f335-38e0-4079-b705-7ef2e5d8406c.png)
위와 같은 연쇄법칙을 활용해 매개변수 y로 이를 계산할 수 있는데 이런 개념을 활용한 것이 back propagation의 핵심   
   
![image](https://user-images.githubusercontent.com/69246778/128969431-9bf74ee3-e1c8-4707-89b8-9c8d3de428e1.png)
즉, 미분을 계산할 때 뒤에서부터 앞으로 오면서 오차와 미분 값들을 곱해주면서 전달. 그러면 중복된 계산을 반복하지 않고 
한 번의 전파로 모든 매개변수에 대한 오차를 전부 계산할 수 있음.
   
Forward propagation은 input에 대한 예측치를 계산하는 거라면, back propagation은 반대로 오차를 가지고 미분값을 계산하는 것임.
신경망 학습에는 forward propagation과 back propagation이 반복되면서 weight가 업데이트 되는 방식으로 학습됨. 
이런 학습 과정에서 고려할 사항은 **over-fitting방지**와 **Gradient Vanishing,Gradient Exploding방지**문제임.

## 6.3. over-fitting방지
### 6.3.A. Early Stopping(조기종료)
손실함수가 더 이상 감소하지 않도록 손실 함수 값을 보고 early termination함.

### 6.3.B. Regularization(규제)
가중치가 너무 크지 않도록 규제함

### 6.3.C. Drop-out 
인공신경망에 있는 하나의 노드에서 계산 결과를 받아들인 다음에 그걸 output으로 내는 과정에서 결과 일부를 일부러 누락시킴. 가중치들을
모두 사용하지 않고도 좋은 예측치를 만들어 내도록 학습하는 효과가 있음. 학습을 하고 test를 할 때는 여러 개의 결과는 종합적으로 
판단하는 앙상블(ensemble)효과가 있음.

### 6.3.D. data augmentation(데이터 증식)
데이터의 패턴을 유지하면서 여러가지 인위적으로 변화된 데이터를 만들어 내는 방식

## 6.4. Gradient vanishing, exploding방지
back propagation을 쓰면 미분값이 뒤에서 앞으로 계속 전달되므로 0에 가까운 값들이 계속 곱해지면서 결과가 매우 작아짐. 
이러면 hidden layer중에서 weight들을 업데이트 하기 위한 미분값이 매우 작아져 학습이 어려워짐.

### 6.4.A. Weight Initialization(가중치 초기화)
### 6.4.B. 활성화함수
LeakyReLU라든가 ELU같이 ReLU의 변형된 형태의 활성화 함수를 쓰면 미분값이 0이 아닌 존재할 수 있도록 해줌,
### 6.4.C. Batch Normalization(배치 정규화)
Batch의 값들이 일정한 범위 내에 들어가도록 하는 방식

# 7. 딥러닝이 강력한 이유
![image](https://user-images.githubusercontent.com/69246778/128971623-fad1c0c6-6243-4eca-9676-0cf628e093d6.png)
feature extraction 자체가 인간의 힘으로 이뤄지던 것이 딥러닝에서는 자체적으로 이뤄짐. layer가 깊어지면 깊어질수록 계층적인 구조를 가지고
있는 특징을 추출할 가능성이 높아짐. '깊은'거 말고 '넓은(wide)'걸로도 생각해 볼 수 있음. wide하다는 것은 한 layer의 뉴런의 수가
굉장히 많아지는 것을 의미함. 


