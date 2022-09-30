---
layout: post
title: 
description: |
  코딩테스트 대비 입출력 형식 정리
hide_image: true
tags:
  - programming
published: True
---

# 코딩테스트 대비 입출력 형식 정리
* * *

# 1. Python

## 1.1. n개의 요소를 갖는 리스트 
```py
n=int(input())
a=list(map(int,input().split()))
```
```py
3
1 2 3
a = [1,2,3]
```

## 1.2. n개의 요소를 갖는 리스트를 T번 받기
```py
T=int(input())
  
for _ in range(T):
    a=list(map(int,input().split()))
    result = sum(a)
    print(result)
```
```py
3
1 2 3
>>6
3 4 5 6
>>18
10 20 30 40 50
>>150
```

# 2.C++
## 2.1. n개의 요소를 갖는 리스트
```cpp
#include <vector>
using namespace std;

int n, num;
vector<int> score;
using namespace std;

int main(int argc, char** argv)
{
	cin >> n ;
	while (n--) {
		cin >> num;
		score.push_back(num);
	}
	printf("%d", score[2]);
	return 0;
}
```
```cpp
3
1 2 3
>> 3
```
