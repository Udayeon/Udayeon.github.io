---
layout: post
title: 
description: |
  Basic Of Coding Test
hide_image: true
tags:
  - programming
published: true
---

# (cpp) Basic Of Coding Test
* * *
# n개의 숫자를 입력받기
```cpp
#include<iostream>
#include<vector>
using namespace std;
int n; 
int num;
vector<int> nlist; //n개의 숫자를 vector로 저장해둘 것

int main() {
	cin >> n; //입력받을 숫자 갯수
	int ret = n; //n번 동안 입력을 반복할 것
	while (ret--) { 
		cin >> num;
		nlist.push_back(num); //입력 받는대로 vector에 저장
	}
	
	for (int i = 0; i < nlist.size(); i++) { cout << nlist[i] << ' '; } //잘 저장되었는지 
}
```
