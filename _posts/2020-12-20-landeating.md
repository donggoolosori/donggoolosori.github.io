---
layout: post
title: "[프로그래머스] 땅따먹기- C++"
subtitle: "level 2, DP"
date: 2020-12-20 19:35:00
author: "dongjune"
header-img: "img/in_post/6.jpg"
catalog: true
tags:
  - Programmers
---
# 문제
[땅따먹기](https://programmers.co.kr/learn/courses/30/lessons/12913)
# 풀이
DP를 사용하여 풀이했습니다. 다음은 DP를 구현한 재귀함수입니다.
```c++
int findMax(int y,int x,vector<vector<int>> &land){
    // 기저사례
    if(y==n-1)
        return land[y][x];
    // 메모이제이션
    int &ret = cache[y][x];
    if(ret!=-1)
        return ret;
    int tempMax=0;
    for(int i=0;i<4;i++){
        if(i==x) continue;
        tempMax = max(tempMax,findMax(y+1,i,land));
    }
    return cache[y][x] = land[y][x] + tempMax;
}
```
cache 배열에 계산 된 값을 저장해서 중복되는 재귀함수의 호출을 막아줍니다.  

# 소스 코드
```c++
#include <iostream>
#include <vector>
#include <cstring>
#include <algorithm>

using namespace std;

int cache[100000][4];
int n;
int findMax(int y,int x,vector<vector<int>> &land){
    // 기저사례
    if(y==n-1)
        return land[y][x];
    // 메모이제이션
    int &ret = cache[y][x];
    if(ret!=-1)
        return ret;
    int tempMax=0;
    for(int i=0;i<4;i++){
        if(i==x) continue;
        tempMax = max(tempMax,findMax(y+1,i,land));
    }
    return cache[y][x] = land[y][x] + tempMax;
}

int solution(vector<vector<int> > land)
{
    n = land.size();
    
    // cache -1로 초기화
    memset(cache,-1,sizeof(cache));
    int answer=0;
    for(int i=0;i<4;i++)
        answer = max(answer,findMax(0,i,land));
    return answer;
}
```