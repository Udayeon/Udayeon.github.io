---
layout: post
title: 
description: |
  파이썬 함수
hide_image: true
tags:
  - programming
published: true
---

# (python) 함수
* * *

# 1. 파이썬 함수

## 1.1. 파이썬 함수의 구조
```py
def 함수명(매개변수):
  <수행할 문장1>
  <수행할 문장2>
  return 결과값
```

## 1.2. 예제
BAEKJOON 15596번   
정수 n개가 주어졌을 때, n개의 합을 구하는 함수를 작성하시오.   
Python 2, Python 3, PyPy, PyPy3: def solve(a: list) -> int   
a: 합을 구해야 하는 정수 n개가 저장되어 있는 리스트 (0 ≤ a[i] ≤ 1,000,000, 1 ≤ n ≤ 3,000,000)   
리턴값: a에 포함되어 있는 정수 n개의 합 (정수)   
   
```py
def solve(a):
    ans = sum(a)
    return ans
```
```py
>>> a=[1,2,3]
>>> solve(a)
6
```
