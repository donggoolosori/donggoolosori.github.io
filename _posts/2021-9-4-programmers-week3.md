---
layout: post
title: "[프로그래머스 위클리 챌린지 3주차] 퍼즐 조각 채우기 - C++"
subtitle: "weekly challenge 3"
date: 2021-9-4 16:10:00
author: "dongjune"
header-img: "img/in_post/3.jpg"
catalog: true
tags:
  - Programmers
---
# 문제
[위클리 챌린지 3주차 - 퍼즐 조각 채우기](https://programmers.co.kr/learn/courses/30/lessons/84021){:target="_blank"}
# 구현
## 첫 시도 => 시간초과
처음 접근할때는 필요없는 경우까지 계산하여 경우의 수를 모두 계산했습니다.
1. 블록의 순서의 모든 경우의 수 (```필요없음```)
2. 4방향의 회전 수 고려 (```모든 방향을 고려할 필요 없음```)
3. 들어갈 수 있는 칸에 넣은 경우와 안 넣은 경우 모두 고려 (```필요없음, 넣을 수 있는 경우에는 무조건 넣는게 맞음```)

위에 경우 처럼 계산하니 당연히 시간초과가 일어났습니다.
## 해결

생각해보니 블록을 채웠을 때 반드시 주변에 빈공간이 생기면 안되는 조건이 있었습니다.  
이 말 뜻은 게임 보드에서 어느 빈 공간은 들어갈 수 있는 블록이 오직 1개만 존재한다는 뜻입니다.  

따라서 블록의 순서를 고려할 필요도 없고, 블록의 모든 방향을 검사할 필요도 없습니다(블록의 하나의 방향이 일치하면 나머지 방향은 검사할 필요가 없습니다.)  

블록들 마다 방향을 회전해가며, 게임 보드에 넣을 수 있는 공간이 있으면 그냥 넣어주고 다음 블록을 검사하는 방식으로 문제를 해결했습니다.  

## 함수
### ```getBlock``` (bfs로 블록의 좌표값 구하기)
### ```getBlocks``` (getBlock 함수를 사용하여 모든 블록의 좌표 구하기)
### ```rotateBlock``` (블록을 시계방향으로 90도 회전)
### ```countFill``` (보드에서 1의 개수 반환)
### ```insertBlock``` (보드에 block 넣기)
### ```removeBlock``` (보드에서 block 빼기)
### ```canInsertBlock``` (block이 들어갈 공간이 비어있는지 체크)
### ```checkAround``` (block 주변에 빈공간 있는지 체크)
### ```doPuzzle``` (block을 최대로 채운 후 1 개수 카운트)

# 코드
```cpp
#include <algorithm>
#include <queue>
#include <string>
#include <vector>

using namespace std;

int dy[] = {-1, 0, 1, 0}, dx[] = {0, 1, 0, -1};
int len;

// bfs로 탐색 후 블록 모양 반환
void getBlock(vector<vector<int>> &table, vector<vector<bool>> &visit,
              vector<vector<int>> &block, int y, int x) {
  queue<vector<int>> q;
  q.push({y, x});
  visit[y][x] = true;

  while (!q.empty()) {
    vector<int> curr = q.front();
    int cur_y = curr[0], cur_x = curr[1];
    q.pop();

    for (int i = 0; i < 4; i++) {
      int ny = cur_y + dy[i], nx = cur_x + dx[i];
      if (ny < 0 || nx < 0 || ny >= len || nx >= len) continue;
      if (visit[ny][nx] || table[ny][nx] == 0) continue;
      visit[ny][nx] = true;
      block.push_back({ny, nx});
      q.push({ny, nx});
    }
  }
}

// 모든 block들 반환
vector<vector<vector<int>>> getBlocks(vector<vector<int>> &table) {
  vector<vector<vector<int>>> blocks;
  vector<vector<bool>> visit(len, vector<bool>(len));

  for (int i = 0; i < len; i++) {
    for (int j = 0; j < len; j++) {
      if (table[i][j] == 0 || visit[i][j]) continue;
      vector<vector<int>> block;
      block.push_back({i, j});

      getBlock(table, visit, block, i, j);
      for (auto &pos : block) {
        pos[0] -= i;
        pos[1] -= j;
      }
      blocks.push_back(block);
    }
  }

  return blocks;
}

// block을 시계방향으로 90도 돌림
vector<vector<int>> rotateBlock(vector<vector<int>> &block) {
  for (auto &piece : block) {
    int temp = piece[0];
    piece[0] = piece[1];
    piece[1] = -temp;
  }
  return block;
}

// 보드가 채워진 개수 반환
int countFill(vector<vector<int>> &game_board) {
  int ret = 0;
  for (int i = 0; i < len; i++)
    for (int j = 0; j < len; j++)
      if (game_board[i][j] == 1) ret++;

  return ret;
}

// block이 들어갈 y,x 좌표가 비어있는지 체크
bool canInsertBlock(vector<vector<int>> &game_board, vector<vector<int>> &block,
                    int y, int x) {
  for (const auto &piece : block) {
    int ny = y + piece[0], nx = x + piece[1];
    if (ny < 0 || nx < 0 || ny >= len || nx >= len) return false;
    if (game_board[ny][nx] != 0) return false;
  }
  return true;
}

// block을 넣은 후 주변에 빈공간 있는지 체크
bool checkAround(vector<vector<int>> &game_board, vector<vector<int>> &block,
                 int y, int x) {
  for (const auto &piece : block) {
    int ny = y + piece[0], nx = x + piece[1];
    for (int i = 0; i < 4; i++) {
      int ay = ny + dy[i], ax = nx + dx[i];
      if (ay < 0 || ax < 0 || ay >= len || ax >= len) continue;
      if (game_board[ay][ax] == 0) return false;
    }
  }
  return true;
}

// 보드에 블록 넣기
void insertBlock(vector<vector<int>> &game_board, vector<vector<int>> &block,
                 int y, int x) {
  for (const auto &piece : block) {
    int ny = y + piece[0], nx = x + piece[1];
    game_board[ny][nx] = 1;
  }
}

// 보드에서 블록 제거
void removeBlock(vector<vector<int>> &game_board, vector<vector<int>> &block,
                 int y, int x) {
  for (const auto &piece : block) {
    int ny = y + piece[0], nx = x + piece[1];
    game_board[ny][nx] = 0;
  }
}

// 가능한 모든 블록을 넣고 최대 값 구하기
void doPuzzle(vector<vector<int>> &game_board,
              vector<vector<vector<int>>> &blocks) {
  for (auto &block : blocks) {
    bool flag = true;
    for (int r = 0; r < 4; r++) {
      if (!flag) break;
      if (r != 0) rotateBlock(block);

      for (int i = 0; i < len; i++) {
        if (!flag) break;

        for (int j = 0; j < len; j++) {
          if (game_board[i][j] == 1) continue;
          if (!canInsertBlock(game_board, block, i, j)) continue;

          insertBlock(game_board, block, i, j);
          if (!checkAround(game_board, block, i, j))
            removeBlock(game_board, block, i, j);
          else {
            flag = false;
            break;
          }
        }
      }
    }
  }
}

int solution(vector<vector<int>> game_board, vector<vector<int>> table) {
  len = game_board.size();
  vector<vector<vector<int>>> blocks = getBlocks(table);
  int initialFill = countFill(game_board);
  doPuzzle(game_board, blocks);

  return countFill(game_board) - initialFill;
}
```