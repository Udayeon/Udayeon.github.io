---
layout: post
title: 
description: |
  파이썬 클래스 상속과 super()
hide_image: true
tags:
  - programming
published: true
---

# (python) Class 상속과 super()
* * *

# 1.파이썬 클래스 상속
```py
class Person:
  def __init__(self,name,age):
    self.name = name
    self.age = age
    
  def get_name(self):
    print(f'제 이름은 {self.name}입니다.')
    
  def get_age(self):
    print(f'제 나이는 {self.age}입니다.')
    
a = Person("dy",27)
a.get_name()
a.get_age()
```
```
>> 제 이름은 dy입니다.
>> 제 나이는 27입니다.
```
위와 같은 클래스에 새로운 속성을 추가로 부여한다면?

```py
class Person_info:
  def __init__(self,name,age, grade):
    self.name = name
    self.age = age
    self.grade = grade
    
  def get_name(self):
    print(f'제 이름은 {self.name}입니다.')
    
  def get_age(self):
    print(f'제 나이는 {self.age}입니다.')
  
  def get_grade(self):
    print(f'제 성적은 {self.grade}입니다.')
  
a = Person_info("dy",27,"A")
a.get_name()
a.get_age()
a.get_grade()
```
그럼 같은 작업을 넘 여러번 반복하게 되므로 비효율적임.

```py
class Person_info(Person):
  def __init__(self,name,age,grade):
    super().__init__(name,age)
    self.grade = grade
  
  def get_grade(self):
    print(f'제 성적은 {self.grade}입니다.')
```
그래서 위와같이 원래 있던거를 상속 받고 새롭게 추가되는 속성에 대해서만 정의 추가해주면 된다.   
부모 클래스에 전달할 input을 super().__init__()내에 적어주면 됨.

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def get_name(self):
        print(f'제 이름은 {self.name}입니다.')

    def get_age(self):
        print(f'제 나이는 {self.age}입니다.')


class Person_info(Person):
    def __init__(self, name, age, grade):
        super().__init__(name, age)
        self.grade = grade


    def get_grade(self):
        print(f'제 성적은 {self.grade}입니다.')


a = Person_info("dy", 27, "A")
a.get_name()
a.get_age()
a.get_grade()
```
```py
제 이름은 dy입니다.
제 나이는 27입니다.
제 성적은 A입니다.
```

# 2.다른 메서드 구현
```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def get_name(self):
        print(f'제 이름은 {self.name}입니다.')

    def get_age(self):
        print(f'제 나이는 {self.age}입니다.')


class Person_info(Person):
    def __init__(self, name, age, grade):
        super().__init__(name, age)
        self.grade = grade

    def get_name(self):
        print(f'저는 {self.name}입니다.')
        
    def get_grade(self):
        print(f'제 성적은 {self.grade}입니다.')

aa = Person("dy",27)
aa.get_name()
a = Person_info("dy", 27, "A")
a.get_name()
```
```
제 이름은 dy입니다.
저는 dy입니다.
```
메서드 이름은 같지만 class에 따라 다른 값이 출력되게 할 수 있음. 
