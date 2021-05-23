---
layout: post
title: "[프로그래머스] 문자열 압축 - C++"
subtitle: "level2"
date: 2021-1-2 18:40:00
author: "dongjune"
header-img: "img/in_post/3.jpg"
catalog: true
tags:
  - Programmers
---
# 문제
[프로그래머스 문자열 압축](https://programmers.co.kr/learn/courses/30/lessons/60057#){:target="_blank"}
# 풀이
다양한 단위 길이로 문자열을 압축했을 때 압축 된 문자열의 가장 최소 길이를 찾는 문제입니다. 

- 주어진 문자열의 길이가 n이라면 문자열을 자를 수 있는 단위 길이는 1 부터 n/2 까지 입니다. n/2를 넘어가서 문자열을 자르면 어차피 같은 문자열이 나올 수 없기 때문입니다.  
- 문자열을 자를 때는 string의 substr 메소드를 사용했습니다.    
  
substr이 느리다고 알고있어서 시간초과가 나지 않을까 걱정했는데 다행히 시간초과 문제는 나타나지 않았고, 오히려 좋아요를 가장 많이 받은 코드보다 2배정도 빠른 것을 확인했습니다.  

이제 코드의 주석을 보며 자세히 살펴봅시다.

# 코드
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(string s) {
    int minLen = 1000;
    int len = s.length();
    
    if(len==1) return 1; // 길이가 1이면 바로 1 return
    
    // unit : 압축단위, init : 자르는 문자들의 첫 인덱스
    for(int unit=1;unit<=len/2;unit++){
        
        string cur = s.substr(0,unit); 
        int cnt=1, zip = len;
        
        for(int init=unit;init<len;init+=unit){
            string temp;

            // 길이가 1이면 굳이 서브스트링을 사용하지 않는다.
            if(unit==1) temp = s[init];
            else temp = s.substr(init, unit);

            // 연속으로 같은 문자면 cnt 증가
            if(temp==cur){
                cnt++;
            // 다른 문자가 오면
            }else{
                cur = temp;
                if(cnt==1) continue;
                zip -= (cnt-1)*unit; // 중복되는 문자열의 길이 빼주기
                zip += to_string(cnt).length(); // 앞에 붙을 숫자의 길이 더해주기
                cnt=1; // cnt 초기화
            }
        }
        // 위 for문의 마지막 순회에서 else 문을 들어가지 못했을 경우가 있으므로
        if(cnt!=1) {
            zip -= (cnt-1)*unit;
            zip += to_string(cnt).length();
        }
        
        minLen = min(minLen, zip);
    }
    
    return minLen;
}
```