---
layout: post
title: 
description: |
  Softeer '바이러스' - 구현
hide_image: true
tags:
  - programming
published: true
---

# (cpp) Softeer '바이러스' - 구현
* * *
[문제](https://softeer.ai/practice/info.do?idx=1&eid=407&sw_prbl_sbms_sn=84518)

# 풀이
```cpp
#include<iostream>
#include<cmath>
using namespace std;
int n;
long long int k,p;

int main()
{
	//바이러스수 K
	//1초당 P배
	//총시간 N
	cin >> k>> p>>n;
	while(n--){
		k=(k*p)%1000000007;	
	}
	printf("%lld",k);
	return 0;
}
```
