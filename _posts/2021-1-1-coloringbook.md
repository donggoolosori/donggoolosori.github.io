---
layout: post
title: "[프로그래머스] 카카오프렌즈 컬러링북 - C++"
subtitle: "level2"
date: 2021-1-1 19:40:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - Programmers
---
# 문제
[프로그래머스 카카오프렌즈 컬러링북](https://programmers.co.kr/learn/courses/30/lessons/1829){:target="_blank"}
# 풀이
DFS를 활용하여 영역의 개수를 세줬습니다. 다음은 dfs를 구현한 재귀함수입니다.
```c++
// y,x : 현재 좌표
// type : 영역의 종류
int countArea(const vector<vector<int>> &picture,int y,int x,int type){
    visit[y][x]=true;
    int ret=1;

    for(int i=0;i<4;i++){
        int ny=y+dy[i];
        int nx=x+dx[i];
        // 영역의 범위를 벗어나거나 방문한 적 있으면 continue
        if(ny<0 || nx<0 || ny>=M || nx>=N || visit[ny][nx]) continue;
        // 영역의 종류가 다르면 continue
        if(picture[ny][nx]!=type) continue;
        
        // 재귀함수 호출
        ret+=countArea(picture,ny,nx,type);
    }

    return ret;
}
```
# 코드
```c++
#include <vector>
#include <cstring>
#include <algorithm>

using namespace std;

int M,N;
int dy[4]={0,-1,0,1};
int dx[4]={-1,0,1,0};
bool visit[100][100];

int countArea(const vector<vector<int>> &picture,int y,int x,int type){
    visit[y][x]=true;
    int ret=1;

    for(int i=0;i<4;i++){
        int ny=y+dy[i];
        int nx=x+dx[i];
        // 영역의 범위를 벗어나거나 방문한 적 있으면 continue
        if(ny<0 || nx<0 || ny>=M || nx>=N || visit[ny][nx]) continue;
        // 영역의 종류가 다르면 continue
        if(picture[ny][nx]!=type) continue;
        
        // 재귀함수 호출
        ret+=countArea(picture,ny,nx,type);
    }

    return ret;
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
vector<int> solution(int m, int n, vector<vector<int>> picture) {
    M=m,N=n;
    // 영역의 개수, 가장 큰 영역의 크기
    int countRegions=0,maxRegion=0;
    
    // 전역변수 visit 초기화
    memset(visit,0,sizeof(visit));

    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            if(!visit[i][j] && picture[i][j]!=0){
                countRegions++;
                maxRegion = max(maxRegion, countArea(picture,i,j,picture[i][j]));
            }
        }
    }
    return {countRegions,maxRegion};
}
```