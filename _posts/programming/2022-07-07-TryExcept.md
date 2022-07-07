---
layout: post
title: 
description: |
  게시물 설명 간략히
hide_image: true
tags:
  - programming
published: true
---

# (Python) try, except 문
* * *
try 블록 수행 중 오류가 발생하면 except 블록이 수행. try에서 오류가 발생하지 않으면 except는 수행되지 않음.

# 1. try, except만 쓰는 방법 
오류 종류에 상관 없이 오류가 발생하면 except가 수행됨.
```py
try:
  ...
except:
  ...
```

# 2. 발생 오류만 포함한 except
지정해 둔 것과 같은 이름의 특정 오류 발생시 except 실행.
```py
try:
  ...
except 발생 오류:
  ...
```

# 3. 발생 오류 & 오류 메세지 변수까지 모두 포함한 except
오류 메세지의 내용까지 알고 싶을 경우.
```py
try:
    ...
except 발생 오류 as 오류 메시지 변수:
    ...
```

## 3.1. 예시
```py
try:
    4 / 0
except ZeroDivisionError as e:
    print(e)
```
4를 0으로 나누면 **ZeroDivisionError**이 발생. 그럼 except 블록이 수행됨.
이 때, 오류 메세지 내용을 저장한 **e**를 print할 수 있음. 

