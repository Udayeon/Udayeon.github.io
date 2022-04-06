---
layout: post
title: 
description: |
  Positinal Encoding 예제
hide_image: true
tags:
  - deeplearning
published: true
---

# (Python) A Positional Encoding Example
* * *


**Step1. 필요한 모듈 불러오기**
```py
!pip install gensim==3.8.3  #word2vec같은 패키지를 포함한 라이브러리
import torch
import nltk	#자연어처리 패키지
nltk.download('punkt') 

import math
import numpy as np
from nltk.tokenize import sent_tokenize, word_tokenize 
import gensim 
from gensim.models import Word2Vec 
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity
import matplotlib.pyplot as plt
import warnings 
warnings.filterwarnings(action = 'ignore') 
```

**Step2.파일열기***
```py
#‘text.txt’ file 
sample = open("text.txt", "r") 
s = sample.read()    #'s'라는 변수 안에 text.txt의 내용들이 들어감.

# processing escape characters 
f = s.replace("\n", " ")   #문자열변경함수, "\n"을 " "으로 변경. 줄바꿈 되어 있던 걸 한 줄에 나오도록...
```
![image](https://user-images.githubusercontent.com/69246778/161967017-6286456c-0329-4fc5-b19b-d4891db98bcd.png)


**Step3.토큰화해서 data만들기**
```py
data = [] 
# sentence parsing 
for i in sent_tokenize(f):      #step2에서 만든 문자열데이터 f에 대해  sent_tokenize 적용. 문장을 각각 묶어줌.
	temp = [] 
	# tokenize the sentence into words 
	for j in word_tokenize(i):  #sent_tokenize를 통해 묶어진 문장마다 word_tokenize적용.
		temp.append(j.lower()) 
	data.append(temp) 
```
**sent_tokenize**   
![image](https://user-images.githubusercontent.com/69246778/161967630-aaf90710-d465-4cf1-a926-67dbd6960543.png)   
   
**word_tokenize**   
![image](https://user-images.githubusercontent.com/69246778/161967699-955b1e19-0ddc-42e6-81e4-42bd670b6b9b.png)
   
**data**
![image](https://user-images.githubusercontent.com/69246778/161967853-bcc16af6-a9c8-4361-b241-a13cab49af01.png)
   
**Step4. skip-gram 모델 형성**
```py
# Creating Skip Gram model 
#model2 = gensim.models.Word2Vec(data, min_count = 1, size = 512,window = 5, sg = 1) 
#model = Word2Vec(sentences=common_texts, vector_size=100, window=5, min_count=1, workers=4)
model2 = gensim.models.Word2Vec(data, min_count = 1, size = 512,window = 5, sg = 1) 

# 1-The 2-black 3-cat 4-sat 5-on 6-the 7-couch 8-and 9-the 10-brown 11-dog 12-slept 13-on 14-the 15-rug.
word1='black'
word2='brown'
pos1=2		#'black'위치
pos2=10		#'brown'위치
a=model2[word1]	#model2를 이용해 word2vec적용
b=model2[word2]
```

**Step5. 유사도 계산**
```py
# compute cosine similarity
dot = np.dot(a, b)
norma = np.linalg.norm(a)
normb = np.linalg.norm(b)
cos = dot / (norma * normb)

aa = a.reshape(1,512) 
ba = b.reshape(1,512)
cos_lib = cosine_similarity(aa, ba) #cosine 유사도
```


```py
pe1=aa.copy()
pe2=aa.copy()
pe3=aa.copy()
paa=aa.copy()
pba=ba.copy()
d_model=512
max_print=d_model
max_length=20

for i in range(0, max_print,2):
                pe1[0][i] = math.sin(pos1 / (10000 ** ((2 * i)/d_model)))
                paa[0][i] = (paa[0][i]*math.sqrt(d_model))+ pe1[0][i]
                pe1[0][i+1] = math.cos(pos1 / (10000 ** ((2 * i)/d_model)))
                paa[0][i+1] = (paa[0][i+1]*math.sqrt(d_model))+pe1[0][i+1]
                if dprint==1:
                        print(i,pe1[0][i],i+1,pe1[0][i+1])
                        print(i,paa[0][i],i+1,paa[0][i+1])
                        print("\n")

#print(pe1)
# A  method in Pytorch using torch.exp and math.log :
max_len=max_length                
pe = torch.zeros(max_len, d_model)
position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
pe[:, 0::2] = torch.sin(position * div_term)
pe[:, 1::2] = torch.cos(position * div_term)
#print(pe[:, 0::2])
```


```py
for i in range(0, max_print,2):
                pe2[0][i] = math.sin(pos2 / (10000 ** ((2 * i)/d_model)))
                pba[0][i] = (pba[0][i]*math.sqrt(d_model))+ pe2[0][i]
            
                pe2[0][i+1] = math.cos(pos2 / (10000 ** ((2 * i)/d_model)))
                pba[0][i+1] = (pba[0][i+1]*math.sqrt(d_model))+ pe2[0][i+1]
               
                if dprint==1:
                        print(i,pe2[0][i],i+1,pe2[0][i+1])
                        print(i,paa[0][i],i+1,paa[0][i+1])
                        print("\n")

print(word1,word2)
cos_lib = cosine_similarity(aa, ba)
print(cos_lib,"word similarity")
cos_lib = cosine_similarity(pe1, pe2)
print(cos_lib,"positional similarity")
cos_lib = cosine_similarity(paa, pba)
print(cos_lib,"positional encoding similarity")

if dprint==1:
        print(word1)
        print("embedding")
        print(aa)
        print("positional encoding")
        print(pe1)
        print("encoded embedding")
        print(paa)

        print(word2)
        print("embedding")
        print(ba)
        print("positional encoding")
        print(pe2)
        print("encoded embedding")
        print(pba)
```

