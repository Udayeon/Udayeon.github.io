---
layout: post
title: 
description: |
  Softeer '8단 변속기'
hide_image: true
tags:
  - programming
published: true
---

# (cpp) Softeer '8단 변속기'
* * *
[문제](https://softeer.ai/practice/info.do?idx=1&eid=408)

# 풀이
```cpp
#include<iostream>
#include<vector>
using namespace std;
int n;
char asc[20] = "ascending";
char des[20] = "descending";
char mix[20] = "mixed";
vector<int> nlist;

int main(int argc, char** argv)
{
	//1~8연속증가 : ascending
	//8~1연속감소 : descending
	//둘다 아니면 : mixed
	//if nlist[0]==1; asc ,nlist[i+1]-nlist[i] ==1; cnt = cnt+1;
	//                     else mixed;
	//if nlist[0]==8; des, nlist[i] - nlist[i+1] ==1; cnt = cnt+1;
	//						else mixed;
	//else mixed;
	for (int i=0; i<8;i++){
		cin >> n;
		nlist.push_back(n);
	}
	
	if (nlist[0] ==1){
		int cnt=0;
		for(int i=0; i<7;i++){
			if(nlist[i+1]-nlist[i] ==1){
				cnt=cnt+1;
			}
		}
		if(cnt==7){printf("%s",asc);}
		else if(cnt!=7){printf("%s",mix);}
		return 0;
	}


	if (nlist[0] ==8){
		int cnt=0;
		for(int i=0; i<7;i++){
			if(nlist[i]-nlist[i+1] ==1){
				cnt=cnt+1;
			}
		}
		if(cnt==7){printf("%s",des);}
		else if(cnt!=7){printf("%s",mix);}
		return 0;
	}


	else{
		printf("%s",mix);
		return 0;
	}
}
```
