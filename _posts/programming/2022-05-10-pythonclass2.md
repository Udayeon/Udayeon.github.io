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

# 1. 파이썬 클래스 상속
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
