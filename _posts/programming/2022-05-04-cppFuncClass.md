---
layout: post
title: 
description: |
  함수 정의,선언,호출,선행처리자,기억클래스
hide_image: true
tags:
  - programming
published: true
---

# (c++) 함수와 기억클래스
* * *

# 1. 함수의 정의,선언,호출
## 1.1. 함수의 정의
```c++
자료형 함수이름(매개변수 리스트)
{
  변수 선언;
  문장;
  return (결과값);
}
```
**예제**
```c++
#include <iostream>
using namespace std;

void show() //void자료형의 show라는 함수 정의
{
  cout<<"***********";
  return;
}

void main(){  //main함수
  cout<<"함수 호출하기 전";
  show(); //show라는 함수를 호출해서 위에서 정의한 것처럼 "***********"이 출력되게 함.
  cout<<"함수 호출한 후";
}
```

**예제 : 두 수의 합을 구하는 함수 만들기**
```c++
#include <iostream>
using namespace std

void sum(int a, int b)  //매개변수 a,b
{
  cout <<"a+b = "<<a+b; //a,b를 더해서 출력하는 함수
}

//실행
void main()
{
  sum(2,1);
}
```







