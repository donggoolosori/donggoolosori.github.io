---
layout: post
title: "[Algorithm] 이분탐색 - C++"
subtitle: "binary search (이분 탐색)"
date: 2020-09-26 13:32:00
author: "dongjune"
header-img: "img/in_post/code-bg.jpg"
catalog: true
tags:
  - Algorithm
---

# 이분탐색

우리는 배열 {2,3,6,8,10,12,13} 에서 13을 찾고자 한다.  
일반적인 탐색 기법으로는 앞에서부터 배열의 요소를 순차적으로 탐색하기 때문에 시간복잡도는 데이터의 개수 만큼이 된다. 즉 **O(n)** 이 된다. 만일 데이터의 개수가 무수히 많아진다면 굉장히 비효율적인 알고리즘이 된다.  
하지만 이분탐색을 사용하면 **O(log n)** 의 시간복잡도로 탐색을 수행 할 수 있다.

이제 본격적으로 이분탐색 알고리즘을 알아보자. 아래와 같이 1~500까지의 데이터에서 62를 찾아보자(데이터셋은 반드시 **정렬** 돼있어야 한다는 것을 주의하자).

1. 우선 **left**는 가장 왼쪽인 1, **right**는 가장 오른쪽인 500이고 mid는 (1+500)/2인 250이다 (소수점은 버린다).  
   ![1단계](/assets/img/binary1.png)
2. 찾고자 하는 값은 62로 mid 값 250보다 작기 때문에 250 이후의 데이터들은 버린다. 즉 left=1, right = 249, mid = (1+249)/2=125로 갱신 된다.  
   ![2단계](/assets/img/binary2.png)
3. 여전히 타겟 값 62는 125보다 작으므로 2번 과정과 마찬가지로 오른쪽 데이터들을 버려준다. 즉 left=1, right=124, mid = (1+124)/2 = 62로 갱신 된다.  
    ![3단계](/assets/img/binary3.png)  
    mid 값이 타겟 값 62와 일치하므로 탐색이 완료 됐다.
   일반적인 탐색이었으면 1부터 순차적으로 탐색하며 62번 째에 탐색이 완료 됐겠지만 이분탐색으로 **단 3번만에** 탐색을 완료했다. 이제 실제 c++ 코드로 이분탐색을 구현해보자.

## 코드 구현

```c++
#include <iostream>

using namespace std;

int main()
{
   // 1~500까지의 배열 형성
   int arr[501];
   for(int i=1;i<501;i++)
      arr[i]=i;

   int target = 62; // 타겟 값
   // 이분 탐색 수행
   int left = 1, right = 500; // left, right 초기화
   while(left<=right)
   {
      int mid = (left+right)/2 // mid 갱신
      // 타겟 값 찾음
      if(mid==target)
      {
         cout<<"Searching Complete!"<<'\n';
         break;
      }
      else if(mid>target)
      {
         right = mid-1;
      }
      else
      {
         left = mid+1;
      }
   }
   return 0;
}
```
