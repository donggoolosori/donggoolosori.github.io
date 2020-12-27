---
layout: post
title: "[Algorithm] Selection Sort (선택 정렬) - C++"
subtitle: ""
date: 2020-12-27 18:10:00
author: "dongjune"
header-img: "img/in_post/7.jpg"
catalog: true
tags:
  - Algorithm
---
# 선택정렬이란?
선택정렬은 제자리 정렬 알고리즘의 하나입니다.
프로세스는 다음과 같습니다.
1. 배열에서 최소값을 찾는다.
2. 찾은 최소값을 맨 앞의 요소와 바꿔준다.
3. 맨 앞을 제외한 값들에서 최소값을 찾고 위와 같은 방법으로 교체해준다.
  

다음의 배열을 선택 정렬을 사용하여 오름차순 정렬해보겠습니다.  
![IMG_A0E07BB3D88F-1](https://user-images.githubusercontent.com/53213397/103168533-51771980-4877-11eb-9438-dd98fa6e7a87.jpeg)
  
우선 첫번째 인덱스에 들어갈 요소를 찾습니다. 아래와 같이 첫번째 인덱스부터 마지막 인덱스까지 탐색하며 최소값을 찾아줍니다.
![IMG_3CE472E5EF28-1](https://user-images.githubusercontent.com/53213397/103168525-4b813880-4877-11eb-8cf7-c63b944bc776.jpeg)
그 후 첫번째 값과 최소값 1의 자리를 바꿔줍니다.
![IMG_452C601A5EAE-1](https://user-images.githubusercontent.com/53213397/103168530-4fad5600-4877-11eb-8623-1c00c1bca3af.jpeg)
  

마찬가지로 두번째 인덱스에 들어갈 요소도 찾아줍니다.
![IMG_868AE70E4B79-1](https://user-images.githubusercontent.com/53213397/103168532-5045ec80-4877-11eb-8a93-e9c9a46b3679.jpeg)  
세번째 요소도 다음과 같이 찾아줍니다.
![IMG_4A5ACC51404A-1](https://user-images.githubusercontent.com/53213397/103168528-4e7c2900-4877-11eb-801c-5256bc304ee8.jpeg)
  
이런 식으로 첫번째부터 마지막 인덱스 까지 순회하며, 그 인덱스에 위치 할 요소들을 찾아주며 정렬하는 알고리즘이 선택정렬 알고리즘입니다.  

이제 C++ 코드로 선택정렬을 구현해보겠습니다.  

***
# 구현 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    vector<int> numbers = {3, 7, 6, 4, 2, 1};
    int len = numbers.size();
    int index = 0;
    for (int i = 0; i < len; i++)
    {
        int minval = 1000; // 최소값 갱신을 위해 큰 수로 초기화
        // 최소값의 인덱스 찾기
        for (int j = i; j < len; j++)
        {
            if (numbers[j] < minval)
            {
                minval = numbers[j];
                index = j;
            }
        }
        // 최소값이 자신이라면 continue
        if (i == index)
            continue;
        // 값 바꿔주기
        int temp = numbers[i];
        numbers[i] = numbers[index];
        numbers[index] = temp;
    }
    // 출력
    for (auto n : numbers)
        cout << n << ' ';

    return 0;
}
```
***
# 시간복잡도
배열의 요소가 N개라고 가정해봅시다.  
우선 첫번째 자리에 올 요소를 찾기 위해서 N개의 요소를 검사합니다.  
이제 두번째 자리에 올 요소를 찾으려면 N-1개가 필요하고 세번째는 N-2, 네번째는 N-3 ... 이 됩니다.  
모두 다 더하면  
N + (N-1) + (N-2) + ... + 1 = N(N-1)/2 가 되고,  
따라서 시간 복잡도는 O(N^2)이 됩니다.  
  
다른 정렬 알고리즘에 비해 구현이 간단하지만 성능은 좋지 않은 알고리즘입니다.