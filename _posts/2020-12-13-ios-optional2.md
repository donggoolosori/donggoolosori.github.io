---
layout: post
title: "[Swift] Optional - 2"
subtitle: "Optional Binding, Chaining an the Nil Coalescing Operator"
date: 2020-12-13 16:20:00
author: "dongjune"
header-img: "img/in_post/ios.jpg"
catalog: true
tags:
  - iOS
  - Swift
---
[지난 포스팅](https://donggoolosori.github.io/2020/12/13/ios-optional/){:target="_blank"}에서 Optional의 개념과 Forced Unwrapping( ! ) 대해서 알아보았습니다. 하지만 Forced Unwrapping 을 사용하려면 Optional 변수 값이 반드시 존재해야 돼서 이것만으로는 nil 값을 다루기에는 한계가 있었습니다.  

이번 포스팅에서는 이러한 문제점을 해결해주는 4가지 방법에 대해서 자세히 알아보겠습니다.

## <span style="color:rgba(0,0,200,0.6)">1. Check for nil value </span>
단순히 if 문을 사용해서 Unwrap 한 변수가 nil 인 경우를 피하는 방법입니다.  
```swift
let optional:String?

// ....

if optional != nil{
    print(optional!) 
}else{
    print("It's nil!")
}
```
하지만 이 방법에는 아주 큰 문제점이 있습니다.  
바로 매번 이렇게 if state를 사용해야 한다는 것입니다. 또한 if 문 안에서 매번 !를 사용하여 Unwrapping 을 해줘야 합니다. 상당히 번거롭죠. 

## <span style="color:rgba(0,0,200,0.6)">2. Optional Binding </span>
다음은 Optional Binding 입니다. 다음의 코드를 살펴보겠습니다.
```swift
if let safeOptional = optional{
    print(safeOptional)
}else{
    print("It's nil")
}
```
1번째 줄이 의미하는 것은 간단합니다. optional이 nil 이 **아니라면** safeOptional 변수가 자동으로 unwrap 된 optional 값이 할당되어 if 문을 수행합니다.
이 경우는 매번 ! 를 사용하여 Unwrap을 해 줄 필요가 없으니 편리합니다.  

하지만 여전히 몇 가지 문제점이 존재합니다. 
우선 if 문을 사용하는 것이 여전히 번거롭습니다.  
또한 optional이 nil인 경우 if 문을 스킵하고 else로 가게 되는데, 이 경우에 else 문 안에서 예외 처리를 해줘야 하기 때문에 코드가 늘어나게 됩니다.  
  
nil 일 경우 default 값을 갖게 한다면 어떨까요? 이를 위해 바로 다음 살펴볼 Nil Coalescing Operator를 사용할 수 있습니다.

## <span style="color:rgba(0,0,200,0.6)">3. Nil Coalescing Operator</span>
Nil Coalescing Operator를 사용해 Optional 값이 nil일 경우에 변수에 default 값을 할당해봅시다.
```swift
let myOptional:String? // Optional 변수
myOptional = nil

let text:String = myOptional ?? "I am the default value"
print(text)
// 출력 : I am the default value
```
위와 같이 **??** 를 사용하여 myOptional이 nil 일 경우 text 값에 default 값을 할당해줬습니다.
## <span style="color:rgba(0,0,200,0.6)">4. Optional Chaining</span>
이제 struct와 class 가 Optional인 경우를 살펴보겠습니다.
```swift
struct MyOptional{
    var property = 123
    func method(){
        print("I am the struct's method")
    }
}

let myOptional:MyOptional? // struct 를 Optional로 선언

myOptional = MyOptional()

print(myOptional.property) // error
```
위와 같이 단순히 myOptional.property 를 출력하고자 한다면 에러가 발생합니다.  
myOptional 은 현재 Optional 타입이기 때문입니다.  

이 경우 다음과 같이 Optional Chaining을 사용합니다.
```swift
struct MyOptional{
    var property = 123
    func method(){
        print("I am the struct's method")
    }
}

let myOptional:MyOptional? // struct 를 Optional로 선언

myOptional = MyOptional()

print(myOptional?.property) // 출력 : Optional(123)
myOptional?.method()        // 출력 : I am the struct's method
```
12,13번째 줄에서
myOptional 이 nil이 아닐 경우에만 myOptional의 안쪽을 살펴봅니다.  

myOptional이 nil일 경우를 보겠습니다.
```swift
myOptional = nil

print(myOptional?.property) // 출력 : nil
myOptional?.method()        // 출력 : 
```
3번째줄에서는 property 값을 확인하지 않기 때문에 nil을 출력합니다.
4번째 줄에서는 method()를 아예 실행하지 않기 때문에 아무것도 출력하지 않습니다.