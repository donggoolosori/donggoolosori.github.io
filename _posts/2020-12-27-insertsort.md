---
layout: post
title: "[Algorithm] Insertion Sort (삽입 정렬) - C++"
subtitle: ""
date: 2020-12-27 20:10:00
author: "dongjune"
header-img: "img/in_post/1.jpg"
catalog: true
tags:
  - Programmers
---

# 삽입정렬
- 1~n 인덱스를 순회하며 그 인덱스의 값이 왼쪽의 어느 위치에 들어가야 할지 탐색
- 왼쪽의 값들은 항상 정렬돼있다는 특징이 있다.
- 정렬이 대부분 되어있는 경우 높은 속도를 보여줌. 

  
다음의 배열을 삽입정렬을 사용하여 정렬해봅시다.
![IMG_0967E9DCCB7D-1](https://user-images.githubusercontent.com/53213397/103169748-f3036880-4881-11eb-9343-b6811bba9a03.jpeg)
  
우선 두번째 인덱스인 7에서부터 시작합니다. 7은 현재 왼쪽 영역에서 알맞은 순서에 위차하기 때문에 그냥 넘어갑니다.
![IMG_4E907B3271A1-1](https://user-images.githubusercontent.com/53213397/103169746-f0a10e80-4881-11eb-8be5-6db9458a54b8.jpeg)
  
3번째 인덱스인 6을 왼쪽 영역안에서 알맞은 위치인 3 바로 다음에 위치시켜줍니다.
![IMG_B80AB321E128-1](https://user-images.githubusercontent.com/53213397/103169750-f39bff00-4881-11eb-855b-db2c5b903926.jpeg)
  
4번째 인덱스인 4를 왼쪽 영역안에서 알맞은 위치인 3 바로 다음으로 이동시킵니다.
![IMG_F8F902A1E194-1](https://user-images.githubusercontent.com/53213397/103169751-f4cd2c00-4881-11eb-80f5-14573bdca855.jpeg)
  
5번째 인덱스인 2를 마찬가지로 알맞은 위치로 이동시켜줍니다.
![IMG_9D661BC140EA-1](https://user-images.githubusercontent.com/53213397/103169854-d61b6500-4882-11eb-941e-f1ab5c16a103.jpeg)
이제 마지막 인덱스까지 이동시켜주면 선택정렬 완료입니다.
![IMG_C2D3325D7597-1](https://user-images.githubusercontent.com/53213397/103169867-eaf7f880-4882-11eb-89fb-35ece1e35264.jpeg)

삽입정렬은 필요 할 때만 정렬을 수행해주는 특성이 있습니다.  
따라서 배열이 **거의 정렬 된 상태** 라면 가장 좋은 효율을 보여주는 알고리즘입니다. 


***
# 구현 코드
```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> array = {3, 7, 6, 4, 2, 1};
    int N = array.size();

    // i = 1 부터 시작
    for (int i = 1; i < N; i++)
    {
        int j = i;
        // 자신보다 작은 수가 나올때까지 왼쪽으로 이동.
        while (j > 0 && array[j - 1] > array[j])
        {
            int temp = array[j - 1];
            array[j - 1] = array[j];
            array[j] = temp;
            j--;
        }
    }

    // 출력 : 1 2 3 4 6 7
    for (auto n : array)
        cout << n << ' ';

    return 0;
}
```
***
# 시간복잡도
최악의 경우라면 N개의 데이터가 있을 때 다음의 연산회수를 수행하게 됩니다.  

1 + 2 + 3 + ... + N-2 + N-1 = N(N-1)/2  

따라서 시간복잡도는 O(N^2)이 됩니다.