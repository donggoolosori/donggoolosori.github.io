---
layout: post
title: "[Algorithm] Merge Sort (병합 정렬) - C++"
subtitle: "Sort"
date: 2021-1-21 18:40:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - Algorithm
---
# 병합정렬
병합정렬은 이전에 포스팅한 [퀵정렬](https://donggoolosori.github.io/2021/01/05/quicksort/){:target="_blank"}과 마찬가지로 분할정복 패러다임을 기반으로 한 정렬 알고리즘입니다.  
다음은 병합정렬의 매커니즘을 보여주는 그림입니다.
<img width="1032" alt="스크린샷 2021-01-21 오후 9 06 38" src="https://user-images.githubusercontent.com/53213397/105351051-5c7f5880-5c2f-11eb-8824-f39dd7e06034.png">
위의 그림을 보면 병합정렬은 분해가 다 이루어진 후 병합을 하면서 정렬이 이루어집니다. 
퀵정렬이 분해를 하면서 정렬이 이루어지는 것과는 반대되는 모습입니다.  

이제 코드를 통해 직접 구현해보도록 하겠습니다.
# 구현
```c++
/*
1. 분할정복 방식의 정렬
2. 재귀적으로 배열을 분할해준다
3. 각각의 파티션들을 합치며 정렬하는 방식
4. n*log(n)의 시간복잡도
*/
#include <iostream>
#include <vector>
using namespace std;

int sorted[7];
vector<int> arr = {38, 27, 43, 9, 3, 82, 10}; // 정렬할 수열

void merge(int left, int right)
{
    int mid = (left + right) / 2;

    int i = left;
    int j = mid + 1;
    int k = left;

    // 두 부분 수열을 오름차순으로 합치는 과정
    while (i <= mid && j <= right)
    {
        if (arr[i] <= arr[j])
            sorted[k++] = arr[i++];
        else
            sorted[k++] = arr[j++];
    }
    // 남은 요소들을 합치는 과정
    int temp = i > mid ? j : i;
    while (k <= right)
        sorted[k++] = arr[temp++];

    for (int i = left; i <= right; i++)
        arr[i] = sorted[i];
}

void mergeSort(int left, int right)
{
    // 더 이상 분할 되지 않을 때까지 분할한 후 
    // merge 함수를 호출하여 오름차순 정렬이 되도록 병합한다.
    if (left < right)
    {
        int mid = (left + right) / 2;
        mergeSort(left, mid);
        mergeSort(mid + 1, right);
        merge(left, right);
    }
}

int main()
{
    // 병합정렬 수행
    mergeSort(0, 6);
    // 결과 출력 : 3 9 10 27 38 43 82
    for (int i = 0; i < 7; i++)
        printf("%d ", arr[i]);

    return 0;
}
```

# 시간복잡도
병합정렬은 우선 배열을 원소가 하나가 될 때까지 분할하는 과정을 거칩니다. 병합정렬의 분할 방식은 아주 단순하고 효율적입니다. 그냥 가운데서 절반으로 나누는 것입니다. 따라서 분할에 걸리는 시간은 O(1)입니다.  
그 후에 분할된 파티션들을 하나의 배열로 병합하는 과정을 거칩니다. 위의 그림에서 보다시피 배열이 1/2씩 분할되면서 병합하는 총 단계의 수는 log(n)개입니다. 그리고 각 부분 단계마다 병합하기 위한 총 시간은 O(n)개가 됩니다.  
따라서 병합정렬의 시간복잡도는 O(nlog(n))이 됩니다.
