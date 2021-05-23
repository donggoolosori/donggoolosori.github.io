---
layout: post
title: "[프로그래머스] 가장 큰 정사각형 찾기- C++"
subtitle: "level 2"
date: 2020-12-20 18:10:00
author: "dongjune"
header-img: "img/in_post/1.jpg"
catalog: true
tags:
  - Programmers
---
# 문제
[가장 큰 정사각형 찾기](https://programmers.co.kr/learn/courses/30/lessons/12905){:target="_blank"}
# 풀이
다음과 같은 입력이 있습니다. 한눈에 봐도 가장 큰 사각형의 크기는 3x3 = 9 입니다.  
9라는 답을 어떻게 도출할 수 있을까요?
![square1](https://user-images.githubusercontent.com/53213397/102710013-a86a7500-42f2-11eb-820e-e6234b312c46.jpg)
2차원 배열의 요소들을 반복문으로 순회합니다.  
이때 값이 0이라면 정사각형을 만들 수 없기 때문에 그냥 지나갑니다.  
1이 나오면 왼쪽, 위, 왼쪽 대각선의 요소들을 검사합니다.  
이때 다음의 코드와 같이 현재 값을 3가지 요소의 최소값에 1을 더해주는 값으로 갱신합니다.
```c++
board[i][j] = 1 + min({board[i-1][j-1], board[i-1][j], board[i][j-1]});
```
다음의 그림은 (1, 2)의 요소가 갱신되는 모습입니다.
![square2](https://user-images.githubusercontent.com/53213397/102710015-ac969280-42f2-11eb-882a-beb7b35b36d3.jpg)
위의 과정을 통해 (2, 2)까지 순회하며 갱신하면 다음의 상태가 됩니다. 이제 (2, 3)을 갱신 할 차례입니다.
![square3](https://user-images.githubusercontent.com/53213397/102710017-adc7bf80-42f2-11eb-8680-a33f0be4dcb8.jpg)
위와 같이 (2, 2, 2)의 최소값 2에 1을 더해 (2, 3)은 3으로 갱신됩니다.  
  
최종적으로 배열안의 최대값은 3이 되고 여기에 제곱을 한 값이 우리가 구하는 값이 됩니다.  

# 소스 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<vector<int>> board)
{
    int answer = board[0][0];
    int r = board.size();
    int c = board[0].size();
    
    for(int i=1;i<r;i++)
    {
        for(int j=1;j<c;j++)
        {
            if(board[i][j]==1){
                board[i][j] = 1 + min({board[i-1][j-1],board[i-1][j],board[i][j-1]});
                answer = max(answer,board[i][j]);
            }
        }
    }

    return answer*answer;
}
```