---
layout: post
title: 
description: |
  선택문, 반복문, 분기문
hide_image: true
tags:
  - programming
published: true
---

# (c++) 제어문
* * *

# 1. 선택문
if문, switch문
## 1.1. if문
```c++
if(조건문1){
  문장1;
}

else if(조건문2){
  문장2;
}

else if(조건문3){
  문장3;
}

else{
  문장n;
}
```
조건문이 참이면 {}안의 문장을 수행하고 거짓이면 그 다음 조건문으로 넘어간다.
또한,실행하려는 문장이 한 개면 {}으로 묶지 않고 두 개이상일 경우에만 {}으로 묶는다.
**예제**
```c++
#include<iostream>  //in,out하기 위한 헤더 파일
using namespace std;
void main()
{
  int score;    //정수형 변수 선언
  char grade;   //문자형 변수 선언
  cout << "점수를 입력하세요 : " ;
  cin >> score;
  
  if(score >=90)
    grade = 'A' ;   //글자 하나는  ''로 묶음
  else if(score >=80)
    grade = 'B' ;
  else if(score >=70)
    grade = 'C' ;
  else if(score >=60)
    grade = 'D' ;
  else
    grade = 'F' ;
  
  cout <<"입력한 점수 " << score << ":" << grade << "학점입니다." ;  //변수 타입 달라지는 부분마다 <<로 끊기
}
```
 
## 1.2. switch문
if문이 조건문의 참,거짓에 따라 문장을 수행하는 것과 달리 switch문은 상수식에 따라 분기된다. 
```c++
switch(정수식){
  case 정수값1 : 문장1;break;
  case 정수값2 : 문장2;break;
  ...
  case 정수값n : 문장n;break;
  default : 문장n+1 ;
}
```

**예제**
```c++
#include <iostream>
using namespace std;
void main()
{
  int score;
  char grade;
  cout << "점수를 입력하세요 :";
  cin >> score; 
  
  switch(score/10) {
    case 10 : grade = 'A'; break;
    case 9 : grade = 'A'; break;
    case 8 : grade = 'B'; break;
    case 7 : grade = 'C'; break;
    case 6 : grade = 'D'; break;
    default : grade = 'F';
  }
  
  cout << "입력한 점수" << score << ":" << grade << "학점입니다.";
}
```

# 2. 반복문
for문, while문, do ~while문
## 2.1. for문
지정된 **횟수**만큼 반복.
```c++
for(초기식; 조건식; 증감식){
  문장1
}
```
**예제**
```c++
#include <iostream>
using namespace std;
void main()
{
  int total = 0;
  for (int i=1; i<=5; i++){       //i를 1씩 올려가면서 i가 5가 될 때까지 {}안의 문장 수행
    cout << "i = " << i << endl;  //endl도 \n처럼 줄바꿈
    total += i;
    cout << "total = " << total;
  }
  
  cout << "1부터 " << i-1 << "까지의 합계는 " << total << "이다."; //i=6으로 넘어가면서 문장을 벗어났음 (현재 i=6인 상태)
}
```

## 2.2. while
조건을 먼저 검사한다. **조건**이 만족되는 동안 반복.
```c++
while(조건식){
  문장;
}
```

**예제**;
```c++
#includ <iostream>
using namespace std;
void main()
{
  int total = 0;
  int i = 1;
  while(i<=10){ //i가 10이 될 때까지
    total += i;
    i++;
  }
  
  cout << "1부터 10까지의 합은 " << total << "이다."; //i=11로 넘어가면서 문장 벗어남 (현재 i=11)
}
```

## 2.3. do~while문
반복되는 문장을 일단 한번은 실행하고 그 다음에 조건이 참이면 계속 반복. 조건검사를 나중에 하므로 조건이 거짓이더라도 일단 한 번은 실행.
```c++
do {
  문장
} while(조건식);
```
**예제**
```c++
#include <iostream>
using namespace std;
void main()
{
  int num;
  do {
      cout << "수를 입력하세요(0을 입력하면 종료) : " ;
      cin >> num;
      cout << num <<"를 입력하셨군여";
      } while(num !=0 );
      cout << num <<"을 입력해서 반복문 종료합니다.";
}
```

# 3. 분기문
반복문과 함께 사용해 더 구체적으로 프로그램을 제어하는 방법
## 3.1. break
프로그램의 일부를 수행하지 않고 건너 뛰게 함.
```c++
while(조건문) {
  문장 1;
  if(조건식)
    break;
  문장2;
}
문장3;
```
while조건이 만족되는 동안 문장1과 문장2를 수행하는데, 문장1이 if조건에서 거짓이면 이땐 문장2를 수행하지 않고문장3으로 빠져나감.

**예제**
```c++
#include <iostream>
using namespace std;
void main()
{
  int total = 0;
  for(int i=1; i <=10; i++)
 {
    if(i%2==0)
      break;
    total +=i;
 }
 cout << "i가 " << i <<"여서 for문을 벗어남" ;
 cout << "total = " <<total;
}
```

## 3.2. continue
```c++
while(조건문){
  문장1;
  if(조건식)
    continue;
  문장2;
}
문장3;
```

**예제**
```c++
#include <iostream>
using namesapce std;
void main()
{
  int total = 0
  for (int i=1; i<=10; i++)
  {
    if(i%2==0)  //i가 2로 나누어 떨어지면 
      continue; //아래 문장은 무시되고 for문의 시작으로 다시 돌아감
    total += i; //홀수일 때만 이 문장을 시행하므로 홀수의 합이 total에 저장됨
  }
  cout <<"i가"<<i<<"일 때 for문을 벗어남";
  cout <<"total:"<<total;
}
```
