---
layout: post
title: 
description: |
  파이썬 클래스
hide_image: true
tags:
  - programming
published: true
---

# (python) Class에 대한 이해
* * *

# 1. Class는 왜 필요한가?
```py
result = 0

def add(num):
    global result
    result += num
    return result

print(add(3))
print(add(4))
```
add라는 함수에 넣은 숫자를 모두 더해준다. 3을 넣으면 이를 기억했다가 후에 넣은 4와 더해준다. 결과는 다음과 같다.
```py
3
7
```
만약 이런 계산기가 2개 필요하다면?
```py
result1 = 0
result2 = 0

def add1(num):
    global result1
    result1 += num
    return result1

def add2(num):
    global result2
    result2 += num
    return result2

print(add1(3))
print(add1(4))
print(add2(3))
print(add2(7))
```
result1과 result2에 대한 2개의 계산기를 만들어야 한다. 두 계산기, add1과 add2는 서로 영향을 미치지 않으므로 결과는 다음과 같다.
```py
3
7
3
10
```
   
근데 만약 계산기가 엄청 많이 필요하다면?
```py
result1 = 0
result2 = 0
.
.
.
resultN = 0

def add1(num):
    global result1
    result1 += num
    return result1

def add2(num):
    global result2
    result2 += num
    return result2
.
.
.
def addN(num):
    global resultN
    resultN += num
    return resultN
```
위와 같이 엄청 많은 함수를 다 다르게 정의해야 한다. 이를 효율적으로 해결하기 위해 Class를 사용한다.
   
```py
class Calculator:   #class명은 Calculator
    def __init__(self):   #초기화를 위한 함수(이 경우 함수를 '메소드'라고도 함)
        self.result = 0

    def add(self, num):   #result는 add함수를 이용해서 구하는데
        self.result += num  #이렇게 계산하고
        return self.result  #출력하는 것이 add함수

cal1 = Calculator()   #cal1은 Calculator 클래스로 만든 하나의 계산기(객체)
cal2 = Calculator()   #cal2도 Calculator 클래스

print(cal1.add(3))    #Calcuator 클래스의 add함수를 사용한다.
print(cal1.add(4))
print(cal2.add(3))
print(cal2.add(7))
```

**클래스 사용의 이점**
- 몇 번이고 재사용할 수 있다.
- 코드 수정이 최소화된다.
- 함수 실행 중에, 함수 자신을 다시 호출할 수 있다.


# __init__과 self
```py
class some_class:
  def __init__(self,something):
    self.something = something
    
  def __init__(self,something):
    print(self.something)
    
  def __init__(self,something):
    return self.something
```
**__init__** 은 **construct** 라고 불리는데 초기화를 위한 함수이다. 인스턴스화를 실시할 떄 반드시 처음에 호출되는
특수한 함수로 인스턴스(오브젝트)생성시 데이터를 초기화 시켜준다.   
**self**는 **인스턴스 자신**을 의미하는데 __init__()의 첫 번째 인수는 반드시 **self**여야한다.

```py
class MyStatus:
  def __init__(self,age,name,height,weight):
    self.age = age
    self.name = name
    self.height = height
    self.weight = weight
    
  def print_name(self):
    print(self.name)
    
  def print_age(self):
    print(self.age)
    
  def print_height(self):
    print(self.height)
    
  def print_weight(self):
    print(self.weight)

a=MyStatus(27,"DY",161,48)
```

    
