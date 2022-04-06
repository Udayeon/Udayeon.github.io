---
layout: post
title: 
description: |
  Attention 구조 
hide_image: true
tags:
  - deeplearning
published: true
---

# (Python) The Attention Mechanism
* * *

```py
import numpy as np
from scipy.special import softmax  #출력층에서 softmax함수 사용
```


**Step1.입력 표현하기**   
모든 입력은 세 가지 표현(Key,Query,Value)를 가져야 한다.
```py
print("Step 1: 3 inputs, d_model=4")  #d_model : 입력벡터 x의 차원 설정(원래는 512인데 4로 축소)
x =np.array([[1.0, 0.0, 1.0, 0.0],    # Input 1 
             [0.0, 2.0, 0.0, 2.0],    # Input 2
             [1.0, 1.0, 1.0, 1.0]])   # Input 3   --> input마다 4차원의 벡터로 만들어준다. 
print(x)
```
![image](https://user-images.githubusercontent.com/69246778/161908699-517a6b2c-a8bf-49e2-87c6-6cb5a7581bb8.png)   
![image](https://user-images.githubusercontent.com/69246778/161909189-51cbd4a1-0ac4-4cf8-8940-4af6bea8d52d.png)


**Step2.가중치 행렬 초기화**  
*Vaswani(2017)* 에서 기술된 가중치 행렬은 64차원이지만 3차원으로 축소한다.  
```py
print("Step 2: weights 3 dimensions x d_model=4")  #4차원 입력벡터 x의 각 요소마다 3차원의 가중치 행렬을 부여한다.

#쿼리
print("w_query")
w_query =np.array([[1, 0, 1],     #input 첫번째 요소에 대한 3차원 Query가중치
                   [1, 0, 0],     #input 두번째 요소에 대한 3차원 Query가중치     
                   [0, 0, 1],     #input 세번째 요소에 대한 3차원 Query가중치
                   [0, 1, 1]])    #input 네번째 요소에 대한 3차원 Query가중치
print(w_query)


#키
print("w_key")
w_key =np.array([[0, 0, 1],     #input 첫번째 요소에 대한 3차원 Key가중치
                 [1, 1, 0],     #input 두번째 요소에 대한 3차원 Key가중치
                 [0, 1, 0],     #input 세번째 요소에 대한 3차원 Key가중치
                 [1, 1, 0]])    #input 네번째 요소에 대한 3차원 Key가중치
print(w_key)


#밸류
print("w_value")
w_value = np.array([[0, 2, 0],     #input 첫번째 요소에 대한 3차원 value가중치
                    [0, 3, 0],     #input 두번째 요소에 대한 3차원 value가중치
                    [1, 0, 3],     #input 세번째 요소에 대한 3차원 value가중치
                    [1, 1, 0]])    #input 네번째 요소에 대한 3차원 value가중치
print(w_value)
```

![image](https://user-images.githubusercontent.com/69246778/161910331-9a2b57b4-3c72-4a2a-9c2a-32467f9eadad.png)   
모델에 가중치 행렬 추가됨




**Step3.Q,K,V를 얻기 위한 행렬 곱셈**
```py
# x는 (3,1,4)차원을 갖고 w_query는 (4,1,3)차원을 가진다.
# 결과는 (3,1,3)
#쿼리
print("Queries: x * w_query")
Q=np.matmul(x,w_query)     #input(3x1x4)과 쿼리가중치행렬(4x1x3)을 곱한다. -->결과 (3x1x3)행렬
print(Q)

#키
print("Keys: x * w_key")
K=np.matmul(x,w_key)     #input(1x4)과 키가중치행렬(4x3)을 곱한다. -->결과 1x3행렬
print(K)

#밸류
print("Values: x * w_value")
V=np.matmul(x,w_value)     #input(1x4)과 밸류가중치행렬(4x3)을 곱한다. -->결과 1x3행렬
print(V)
```
![image](https://user-images.githubusercontent.com/69246778/161913463-858301a9-06b3-4f67-a754-896ba01008a6.png)   
![image](https://user-images.githubusercontent.com/69246778/161913509-1bb2da50-2ea1-4108-b724-e020b48c3da7.png)   
![image](https://user-images.githubusercontent.com/69246778/161913537-5ac95f67-f431-4e6b-a219-316f1bb4dff4.png)   

|input|Q|K|V|
|-----|-|-|-|
|input1|[1,0,2]|[0,1,1]|[1,2,3]|
|input2|[2,2,2]|[4,4,0]|[2,8,0]|
|input3|[2,1,3]|[2,3,1]|[2,6,3]|


**Step4. 스케일링된 어텐션 점수** 
![image](https://user-images.githubusercontent.com/69246778/161914543-1b83e6f5-93fc-4ab5-b65d-7cb32da406b3.png)

```py
print("Step 4: Scaled Attention Scores")
k_d=1   #square root of k_d=3 rounded down to 1 for this example
attention_scores = (Q @ K.transpose())/k_d
print(attention_scores)
```

**Step5. 각 벡터에 대한 스케일링 된 softmax어텐션 점수(step4를 소프트맥스 함수에 대입)**
```py
attention_scores[0]=softmax(attention_scores[0])
attention_scores[1]=softmax(attention_scores[1])
attention_scores[2]=softmax(attention_scores[2])
print(attention_scores[0])
print(attention_scores[1])
print(attention_scores[2])
```

**Step6. 최종 어텐션 표현(step5에 V곱하기), input1,2,3모두 **
```py
print("input1 Attention 1")
input1_attention1=attention_scores[0].reshape(-1,1)
input1_attention1=attention_scores[0][0]*V[0]
print(input1_attention1)

print("input1 Attention 2")
input1_attention2=attention_scores[0][1]*V[1]
print(input1_attention2)

print("input1 Attention 3")
input1_attention3=attention_scores[0][2]*V[2]
print(input1_attention3)




print("input2 Attention 1")
input2_attention1=attention_scores[1].reshape(-1,1)
input2_attention1=attention_scores[1][0]*V[0]
print(input2_attention1)

print("input2 Attention 2")
input2_attention2=attention_scores[1][1]*V[1]
print(input2_attention2)

print("input2 Attention 3")
input2_attention3=attention_scores[1][2]*V[2]
print(input2_attention3)




print("input3 Attention 1")
input3_attention1=attention_scores[2].reshape(-1,1)
input3_attention1=attention_scores[2][0]*V[0]
print(input3_attention1)

print("input3 Attention 2")
input3_attention2=attention_scores[2][1]*V[1]
print(input3_attention2)

print("input3 Attention 3")
input3_attention3=attention_scores[2][2]*V[2]
print(input3_attention3)
```


**Step7. 결과합산**
```py
attention_input1=input1_attention1+input1_attention2+input1_attention3    #input1에 대한
print(attention_input1)

attention_input2=input2_attention1+input2_attention2+input2_attention3    #input1에 대한
print(attention_input2)

attention_input3=input3_attention1+input3_attention2+input3_attention3    #input1에 대한
print(attention_input3)
```
![image](https://user-images.githubusercontent.com/69246778/161916678-87044908-79e6-4a11-9de0-4e23a380a1e8.png)


**Step8. d_model을 64차원으로 확장**
```py
#We assume we have 3 results with learned weights (they were not trained in this example)
#We assume we are implementing the original Transformer paper. We will have 3 results of 64 dimensions each
attention_head1=np.random.random((3, 64))  #64차원 결과 3개를 갖는다고 가정
print(attention_head1)
```



**Step9. 어텐션 서브레이어 헤드들의 출력**
```py
z0h1=np.random.random((3, 64))
z1h2=np.random.random((3, 64))
z2h3=np.random.random((3, 64))
z3h4=np.random.random((3, 64))
z4h5=np.random.random((3, 64))
z5h6=np.random.random((3, 64))
z6h7=np.random.random((3, 64))
z7h8=np.random.random((3, 64))
print("shape of one head",z0h1.shape,"dimension of 8 heads",64*8)
```


**Step10. 헤드 출력 잇기**
```py
print("Step 10: Concatenation of heads 1 to 8 to obtain the original 8x64=512 output dimension of the model")
output_attention=np.hstack((z0h1,z1h2,z2h3,z3h4,z4h5,z5h6,z6h7,z7h8))
print(output_attention)
```

```py
#@title Transformer Installation
!pip -qq install transformers


######Step12. 영어를 프랑스어로 번역######
from transformers import pipeline     #Transformer에서 pipeline모듈 import
translator = pipeline("translation_en_to_fr") #en to fr옵션 선택
#One line of code!
print(translator("It is easy to translate languages with transformers", max_length=40)) 
```
