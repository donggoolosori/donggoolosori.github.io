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
DP를 사용하여 풀이했습니다.  
다음은 ```bottom up``` 방식의 재귀함수입니다.
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
다음은 ```top down``` 방식의 재귀함수입니다.
```c++
int findMaxScore(int y, int x, const vector<vector<int>> &land)
{
    if (y == 0)
        return land[y][x];
    int &ret = cache[y][x];
    if (ret != 0)
        return ret;
    int temp = 0;
    for (int i = 0; i < 4; i++)
    {
        if (i == x)
            continue;
        temp = max(temp, findMaxScore(y - 1, i, land));
    }
    return ret = land[y][x] + temp;
}
```

# 소스 코드
### bottom up
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
### top down
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int cache[100000][4];

int findMaxScore(int y, int x, const vector<vector<int>> &land)
{
    if (y == 0)
        return land[y][x];
    int &ret = cache[y][x];
    if (ret != 0)
        return ret;
    int temp = 0;
    for (int i = 0; i < 4; i++)
    {
        if (i == x)
            continue;
        temp = max(temp, findMaxScore(y - 1, i, land));
    }
    return ret = land[y][x] + temp;
}

int solution(vector<vector<int>> land)
{
    int answer = 0;
    int n = land.size();

    for (int i = 0; i < 4; i++)
        answer = max(answer, findMaxScore(n - 1, i, land));

    return answer;
}
```