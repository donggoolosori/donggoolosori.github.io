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
한 부분배열에서 ```pivot```을 정한 후 pivot보다 작은 수를 왼쪽으로, 큰 수를 오른쪽으로 놓는 과정을 재귀적으로 수행합니다.  

다음의 그림은 퀵 정렬의 동작 과정을 간단하게 보여줍니다.  
![quicksort](https://user-images.githubusercontent.com/53213397/103615754-7b62c700-4f6e-11eb-8b46-8586d1fc3538.jpeg)
- 각 부분수열의 ```pivot(기준)```을 정한다. 
  - pivot을 정할 때 중앙에 가까운 값을 선택하여 비슷한 크기로 분할될수록 성능이 좋아지지만, 여기서는 구현의 편리함을 위해 그냥 맨 처음 수를 pivot으로 정한다.
- 기준보다 작은 수를 왼쪽으로,큰 수를 오른쪽으로 가게끔 수열을 분해한다.
- 그림에서 빨간색 수들은 이전 수를 분할 하는 데 사용된 기준을 표현한다.

  
이제 코드를 통해 직접 구현해보겠습니다.
# 구현
```c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> arr = {38, 27, 43, 9, 3, 82, 10};

void quick_sort(int start, int end) {
  if (start >= end) return;
  int pivot = start;  // 맨 처음 수를 pivot으로 설정
  int low = start + 1, high = end;

  while (low <= high) {
    while (arr[low] <= arr[pivot] && low <= end) low++;
    while (arr[high] >= arr[pivot] && high > start) high--;

    if (low > high)  // 엇갈린 상태면 pivot과 high 교체
      swap(arr[pivot], arr[high]);
    else  // 엇갈리지 않았으면 low high 교체
      swap(arr[low], arr[high]);
  }
  // 재귀 호출
  quick_sort(start, high - 1);
  quick_sort(high + 1, end);
}

// 모든 원소 출력
void print() {
  for (const int &n : arr) cout << n << ' ';
}

int main() {
  // 퀵정렬 수행
  quick_sort(0, 6);
  // 결과 출력 : 3 9 10 27 38 43 82
  print();

  return 0;
}
```
# 시간복잡도
퀵 정렬에서 대부분의 시간을 차지하는 것은 수열을 pivot 값을 기준으로 부분 수열로 나누는 과정입니다.  
퀵정렬의 경우 나눠지는 두 부분 수열이 비슷한 크기로 나눠진다고 보장할 수 없습니다.  
따라서 만약 기준 값이 수열의 최소 또는 최대값일 경우 부분 수열의 크기가 1씩만 줄어드는 현상이 발생하므로, 이런 최악의 경우 시간복잡도는 ```O(N^2)```이 됩니다.  
하지만 평균적으로 부분 문제가 절반에 가깝게 나눠질 경우 시간복잡도는 ```O(nlgn)```이 됩니다.  
그래서 퀵정렬에는 가능한 한 절반에 가까운 분할을 얻기 위해 좋은 pivot 값을 뽑는 다양한 방법들을 사용합니다.  
정리하면 퀵정렬은 ```중간에 가까운 pivot을 선택하는 방법```을 적용하기 때문에 평균적으로 빠른 속도를 낼 수 있고, 시간복잡도는 ```O(nlgn)``` 입니다.  
심지어 퀵정렬은 같은 big O를 갖는 다른 정렬 알고리즘에 비해 빠른 속도를 보입니다. 
그 이유는 데이터의 이동이 데이터의 비교에 비해 현저히 적게 일어나고, 병합정렬처럼 별도의 메모리 공간이 필요하지 않기 때문이라고 합니다.