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

## 1.2. 함수의 선언
함수의 머리 부분만 기술하는 것. 함수를 선언하는 건 함수 호출 시에 컴파일러가 매개변수의 형과 개수를 확인하기 위함이다.  
함수를 호출하는 측과 정의된 측 사이에 데이터를 주고 받기 위해서는 데이터 수와 자료형이 일치해야 하므로 이를 검사하기 위해

```c++
자료형 함수이름(매개변수 리스트);
```






