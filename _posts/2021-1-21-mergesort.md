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
병합정렬은 이전에 포스팅한 [퀵정렬](https://donggoolosori.github.io/2021/01/05/quicksort/){:target="_blank"}과 마찬가지로 ```분할정복 패러다임```을 기반으로 한 정렬 알고리즘입니다.  
다음은 병합정렬의 매커니즘을 보여주는 그림입니다.
<img width="1032" alt="스크린샷 2021-01-21 오후 9 06 38" src="https://user-images.githubusercontent.com/53213397/105351051-5c7f5880-5c2f-11eb-8824-f39dd7e06034.png">
위의 그림을 보면 병합정렬은 분해가 다 이루어진 후 ```병합을 하면서 정렬```이 이루어집니다. 
퀵정렬이 분해를 하면서 정렬이 이루어지는 것과는 반대되는 모습입니다.  
분해과정은 재귀함수로 구현해주고 더 이상 분해할 수 없으면(부분배열의 원소가 1개) 그때부터 정렬을 수행하며 병합하게 됩니다.

### 병합 과정
병합하는 과정에서 임시적으로 정렬 된 값을 보관 할 배열이 필요하게 됩니다.  
두 부분배열이 병합할 때 작은 값부터 차례대로 임시배열에 채워지게 되고, 원본배열의 값을 임시배열 값으로 교체해줍니다.  
아래의 코드는 병합을 수행해주는 함수입니다.
```c++
void merge(int left, int right) {
  int mid = (left + right) / 2;
  // lIdx: 왼쪽 부분배열의 인덱스, rIdx: 오른쪽 부분배열의 인덱스, k: 현재까지 임시배열에 채워진 인덱스
  int lIdx = left, rIdx = mid + 1, k = left;

  while (k <= right) {
    if (lIdx > mid) {
      temp[k++] = arr[rIdx++];
      continue;
    }
    if (rIdx > right) {
      temp[k++] = arr[lIdx++];
      continue;
    }
    if (arr[lIdx] <= arr[rIdx])
      temp[k++] = arr[lIdx++];
    else
      temp[k++] = arr[rIdx++];
  }
  for (int i = left; i <= right; i++) arr[i] = temp[i];
}
```
```k```가 left부터 시작하여 right 인덱스에 도달할때까지 while문을 수행합니다.  
```lIdx```는 왼쪽 부분배열의 시작인덱스 ```rIdx```는 오른쪽 부분배열의 시작인덱스입니다.  
이때 두 부분배열은 이미 재귀적으로 정렬 된 상태이기 때문에 ```lIdx```위치의 값과 ```rIdx``` 위치의 값을 비교하며 그 중 작은 값을 임시벡터 ```temp```에 차례대로 채워넣습니다.  
이 과정에서 left인덱스부터 right인덱스의 값들은 정렬되게 됩니다.

이제 전체 코드를 확인해보겠습니다.  

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

vector<int> temp(7);
vector<int> arr = {38, 27, 43, 9, 3, 82, 10};

void merge(int left, int right) {
  int mid = (left + right) / 2;
  // lIdx: 왼쪽 부분배열의 인덱스, rIdx: 오른쪽 부분배열의 인덱스, k: 현재까지
  // 병합 한 인덱스
  int lIdx = left, rIdx = mid + 1, k = left;

  while (k <= right) {
    if (lIdx > mid) {
      temp[k++] = arr[rIdx++];
      continue;
    }
    if (rIdx > right) {
      temp[k++] = arr[lIdx++];
      continue;
    }
    if (arr[lIdx] <= arr[rIdx])
      temp[k++] = arr[lIdx++];
    else
      temp[k++] = arr[rIdx++];
  }
  for (int i = left; i <= right; i++) arr[i] = temp[i];
}

// 더 이상 분할 되지 않을 때까지 분할한 후
// merge 함수를 호출하여 병합한다.
void merge_sort(int left, int right) {
  if (left >= right) return;
  int mid = (left + right) / 2;
  merge_sort(left, mid);
  merge_sort(mid + 1, right);
  merge(left, right);
}
// 모든 원소 출력
void print() {
  for (const int &n : arr) cout << n << ' ';
}

int main() {
  // 병합정렬 수행
  merge_sort(0, 6);
  // 결과 출력 : 3 9 10 27 38 43 82
  print();

  return 0;
}
```

# 시간복잡도
병합정렬은 우선 배열을 원소가 하나가 될 때까지 분할하는 과정을 거칩니다. 병합정렬의 분할 방식은 아주 단순하고 효율적입니다. 그냥 가운데서 절반으로 나누는 것입니다. 따라서 분할에 걸리는 시간은 ```O(1)```입니다.  
그 후에 분할된 파티션들을 하나의 배열로 병합하는 과정을 거칩니다. 위의 그림에서 보다시피 배열이 1/2씩 분할되면서 병합하는 총 단계의 수는 ```log(n)```개입니다. 그리고 각 부분 단계마다 병합하기 위한 총 시간은 ```O(n)```개가 됩니다.  
따라서 병합정렬의 시간복잡도는 ```O(nlog(n))```이 됩니다.
