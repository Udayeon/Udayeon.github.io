---
layout: post
title: 
description: |
  (c++) 템플릿
hide_image: true
tags:
  - programming
published: True
---

# () Template
* * *

# 1. 템플릿이란
매개변수의 타입에 따라 함수나 클래스를 생선하는 매커니즘. 그니까 여러 자료형에 사용할 수 있도록 일반화된 틀을 만들어 놓은 것.   
필요한 타입을 넣어서 사용하면 된다.

# 1.1. 함수템플릿
함수를 만들 때, 함수의 기능은 명확한데 자료형은 확정짓지 않은 채로 두는 것.
```cpp
int sum(int a, int b){
  return a + b;
}
double sum(double a, double b){
  return a + b;
}
```
템플릿이 없으면 위와 같이 특정 자료형마다 함수를 여러개 작성해야한다. 
```cpp
template <typename T>
T sum(T a,T b){
  return a + b;
}
```
템플릿을 쓰면 위와 같이 한 번만 정의하면 된다. 
   
만약 인자를 2개 받을 건데 둘의 타입이 다르다면 다음과 같다. 클래스가 다른 두 개를 같이 계산할 수 있다.
```cpp
template <class T1, class T2>
void sum(T1 a, T2 b){
  return a + b;
}
```

