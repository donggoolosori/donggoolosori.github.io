---
layout: post
title: "[Algorithm] Bubble Sort (버블 정렬) - C++"
subtitle: "Sort"
date: 2020-12-27 20:50:00
author: "dongjune"
header-img: "img/in_post/6.jpg"
catalog: true
tags:
  - Algorithm
---
# 버블정렬
- 버블정렬은 가장 느린 정렬 알고리즘
- 매 사이클마다 가장 큰 수를 뒤로 보낸다

다음은 버블정렬을 쉽게 설명해놓은 그림입니다.  

![스크린샷 2020-12-27 오후 8 59 23](https://user-images.githubusercontent.com/53213397/103170309-b25a1e00-4886-11eb-834a-1c3358c58a5f.png)

매 pass가 끝날 때 마다 큰 수부터 차례대로 뒤에 채워지는 것을 볼 수 있습니다.
***

# 구현
```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> array = {3, 7, 6, 4, 2, 1};
    int N = array.size();

    for (int i = 0; i < N - 1; i++)
    {
        // 옆에 있는 수를 비교하여 큰 수를 뒤로보내기
        for (int j = 0; j < N - 1 - i; j++)
        {
            if (array[j] > array[j + 1])
            {
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
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
버블정렬은 항상 일정하게 다음의 연산 횟수를 갖습니다.

N-1 + N-2 + ... + 2 + 1 = N(N-1)/2  
  
따라서 시간복잡도는 O(N^2)이 됩니다.