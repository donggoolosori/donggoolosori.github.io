---
layout: post
title: "[Algorithm] Quick Sort (퀵 정렬) - C++"
subtitle: "Sort"
date: 2021-1-5 15:40:00
author: "dongjune"
header-img: "img/in_post/1.jpg"
catalog: true
tags:
  - Algorithm
---
# 퀵정렬
퀵정렬은 분할정복 패러다임을 기반으로 한 정렬 알고리즘입니다.  

다음의 그림은 퀵 정렬의 동작 과정을 간단하게 보여줍니다.  
![quicksort](https://user-images.githubusercontent.com/53213397/103615754-7b62c700-4f6e-11eb-8b46-8586d1fc3538.jpeg)
- 각 부분수열의 맨 처음에 있는 수를 **기준**으로 삼는다.
- 기준보다 작은 수를 왼쪽으로,큰 수를 오른쪽으로 가게끔 수열을 분해한다.
- 그림에서 빨간색 수들은 이전 수를 분할 하는 데 사용된 기준을 표현한다.

  
이제 코드를 통해 직접 구현해보겠습니다.
# 구현
```c++
#include <vector>
#include <iostream>

using namespace std;

void quickSort(int *data, int start, int end)
{
    if (start >= end)
        return;
    int pivot = start; // 기준 값
    int i = start + 1;
    int j = end;

    while (i <= j)
    {
        while (data[i] <= data[pivot]) // 키 값보다 큰 값 만날때까지 오른쪽으로 이동
            i++;
        while (data[j] >= data[pivot] && j > start) // 키 값보다 작은 값 만날 때까지 왼쪽으로 이동
            j--;
        if (i > j) //현재 엇갈린 상태면 pivot 값 교체
        {
            int temp = data[j];
            data[j] = data[pivot];
            data[pivot] = temp;
        }
        else
        {
            int temp = data[j];
            data[j] = data[i];
            data[i] = temp;
        }
        // 재귀 호출
        quickSort(data, start, j - 1);
        quickSort(data, j + 1, end);
    }
}

int main()
{
    int data[7] = {38, 27, 43, 9, 3, 82, 10};
    int len = 7;
    quickSort(data, 0, len - 1);

    for (int i = 0; i < len; i++)
        cout << data[i] << ' ';

    return 0;
}
```
# 시간복잡도
퀵 정렬에서 대부분의 시간을 차지하는 것은 수열을 pivot 값을 기준으로 부분 수열로 나누는 과정입니다.  
퀵정렬의 경우 나눠지는 두 부분 수열이 비슷한 크기로 나눠진다고 보장할 수 없습니다.  
따라서 만약 기준 값이 수열의 최소 또는 최대값일 경우 부분 수열의 크기가 1씩만 줄어드는 현상이 발생하므로, 이런 최악의 경우 시간복잡도는 O(N^2)이 됩니다.  
하지만 평균적으로 부분 문제가 절반에 가깝게 나눠질 경우 시간복잡도는 O(nlgn)이 됩니다.  
그래서 퀵정렬에는 가능한 한 절반에 가까운 분할을 얻기 위해 좋은 pivot 값을 뽑는 다양한 방법들을 사용합니다.