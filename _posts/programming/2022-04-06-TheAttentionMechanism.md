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


######Step1.######
print("Step 1: 3 inputs, d_model=4") #d_model : 몇 차원의 벡터로 만들어 줄지 결정. original에서는 512사용.
x =np.array([[1.0, 0.0, 1.0, 0.0],   # Input 1 
             [0.0, 2.0, 0.0, 2.0],   # Input 2
             [1.0, 1.0, 1.0, 1.0]])  # Input 3   --> input마다 4차원의 벡터로 만들어준다. 
print(x)
```


```py
######Step2.쿼리,키,밸류 가중치 설정하기######
print("Step 2: weights 3 dimensions x d_model=4")

#쿼리
print("w_query")
w_query =np.array([[1, 0, 1],
                   [1, 0, 0],
                   [0, 0, 1],
                   [0, 1, 1]])
print(w_query)


#키
print("w_key")
w_key =np.array([[0, 0, 1],
                 [1, 1, 0],
                 [0, 1, 0],
                 [1, 1, 0]])
print(w_key)


#밸류
print("w_value")
w_value = np.array([[0, 2, 0],
                    [0, 3, 0],
                    [1, 0, 3],
                    [1, 1, 0]])
print(w_value)
```

```py
######Step3.쿼리,키,밸류 계산하기######
#쿼리
print("Step 3: Matrix multiplication to obtain Q,K,V")

print("Keys: x * w_key")  #x:input matrix
K=np.matmul(x,w_key)
print(K)

#키
print("Keys: x * w_key")
K=np.matmul(x,w_key)
print(K)

#밸류
print("Values: x * w_value")
V=np.matmul(x,w_value)
print(V)
```


```py
######Step4.Attention Score계산######
print("Step 4: Scaled Attention Scores")
k_d=1   #square root of k_d=3 rounded down to 1 for this example
attention_scores = (Q @ K.transpose())/k_d
print(attention_scores)
```


```py
######Step5.Attention Score를 softmax함수에######
print("Step 5: Scaled softmax attention_scores for each vector")
attention_scores[0]=softmax(attention_scores[0])
attention_scores[1]=softmax(attention_scores[1])
attention_scores[2]=softmax(attention_scores[2])
print(attention_scores[0])
print(attention_scores[1])
print(attention_scores[2])
```
```py
######Step6. Attention값 구하기######
print("Step 6: attention value obtained by score1/k_d * V")
print(V[0])
print(V[1])
print(V[2])
print("Attention 1")
attention1=attention_scores[0].reshape(-1,1)
attention1=attention_scores[0][0]*V[0]
print(attention1)

print("Attention 2")
attention2=attention_scores[0][1]*V[1]
print(attention2)

print("Attention 3")
attention3=attention_scores[0][2]*V[2]
print(attention3)
```


```py
######Step7. output matrix 첫 줄을 위해 결과 계산 ######
print("Step 7: summed the results to create the first line of the output matrix")
attention_input1=attention1+attention2+attention3
print(attention_input1)
```
```py
######Step8. input 3개에 대해 step1~step7적용하기######
#We assume we have 3 results with learned weights (they were not trained in this example)
#We assume we are implementing the original Transformer paper. We will have 3 results of 64 dimensions each
attention_head1=np.random.random((3, 64))  #64차원 결과 3개를 갖는다고 가정
print(attention_head1)
```
```py
######Step9. 8개의 어텐션 서브레이어 head를 훈련 ######
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
```py
######Step10. output 512차원을 얻기 위해 head 1부터 8까지 연결######
print("Step 10: Concatenation of heads 1 to 8 to obtain the original 8x64=512 output dimension of the model")
output_attention=np.hstack((z0h1,z1h2,z2h3,z3h4,z4h5,z5h6,z6h7,z7h8))
print(output_attention)
```
```py
######Step11. Transformer설치######
#@title Transformer Installation
!pip -qq install transformers
```
```py
######Step12. 영어를 프랑스어로 번역######
from transformers import pipeline     #Transformer에서 pipeline모듈 import
translator = pipeline("translation_en_to_fr") #en to fr옵션 선택
#One line of code!
print(translator("It is easy to translate languages with transformers", max_length=40)) 
```
