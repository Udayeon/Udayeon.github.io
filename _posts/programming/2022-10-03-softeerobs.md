---
layout: post
title: 
description: |
  Softeer '장애물 인식 프로그램' - DFS
hide_image: true
tags:
  - programming
published: true
---

# (cpp) Softeer '지도 자동 구축' -DFS
* * *
[문제](https://softeer.ai/practice/info.do?idx=1&eid=409&sw_prbl_sbms_sn=84468)

# 풀이
```cpp
#include <cstdio>
#include <set>
#include<vector>
#include<algorithm>

using namespace std;

int N; //지도 크기
int map[25][25]; //지도 정보
int dx[4]={-1,1,0,0}; //x방향 탐색
int dy[4]={0,0,1,-1}; //y방향 탐색
bool visit[25][25]; //방문한 곳check


int dfs(int x,int y){
    visit[x][y]=true; //(x,y)를 방문--> true 체크
    int ret=1; //장애물군집 1로 시작

    for(int i=0;i<4;i++){
		//현재 위치(x,y)에 대해 주변 차례대로 탐색
		//(x-1,y) (x+1,y) (x,y+1) (x,y-1) 
        int nx=x+dx[i],ny=y+dy[i];
		//탐색한곳이 지도를 벗어나거나, 장애물이 없거나, 방문한적 있으면 주변탐색을 계속함
		//탐색한곳이 지도 내에 있고, 장애물이 있고, 방문한적 없으면 그 위치로 이동해서 다시 dfs
		//이때 ret이 추가되는데 이게 그 군집내 장애물 갯수가됨.
        if(nx<0||ny<0||nx>=N||ny>=N||!map[nx][ny]||visit[nx][ny]) continue;
        ret+=dfs(nx,ny);
    }
    return ret;
}
int main(){
    scanf("%d",&N);
    for(int i=0;i<N;i++)
        for(int j=0;j<N;j++) scanf("%1d",&map[i][j]);

    vector<int> ans;
    for(int i=0;i<N;i++)
        for(int j=0;j<N;j++)
			//현재 위치에 장애물이 있고, 방문한 적 없으면 그 지점부터 dfs시작.
            if(map[i][j] && !visit[i][j]) ans.push_back(dfs(i,j));
    printf("%d\n",ans.size()); //dfs완료한 횟수 [ret7,ret8,ret9],dfs함수 몇바퀴 돌았는지.
    
    sort(ans.begin(),ans.end());
	for (int i=0; i<ans.size();i++){
		printf("%d\n",ans[i]);}

    
}
```
