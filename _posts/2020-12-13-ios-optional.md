---
layout: post
title: "[Swift] Optional - 1"
subtitle: "?!"
date: 2020-12-13 15:56:00
author: "dongjune"
header-img: "img/in_post/ios.jpg"
catalog: true
tags:
  - iOS
  - Swift
---
### <span style="color:rgba(0,0,200,0.4)">Optional 이란?</span>
Swift는 다른 언어보다 **안전한 코딩**을 하도록 도와줍니다.  
그 안전한 코딩의 중요한 요소중 하나가 오늘 다룰 **Optional** 입니다.  
  
Swift는 기본적으로 변수 선언 시 nil 값이 들어가는 것을 허용하지 않습니다. 다음의 코드에서는 **컴파일 에러**가 일어납니다.
```swift
var myValue : Int = nil // compile error
```
  
그렇다면 Swift에서는 변수 선언 시에 항상 nil 값이 아닌 값을 할당해줘야 할까요?  
  

Swift 에서 nil 값을 갖는 변수를 선언하기 위해서는**Optional**을 사용해야 합니다. 다음은 optional을 사용해서 변수를 선언한 코드입니다.
```swift
var optionalValue : Int? 
```
간단하게 변수 Type 뒤에 ? 를 붙여서 Optional을 선언할 수 있습니다.  
  
Optional은 다음과 같은 의미를 갖습니다.
> 이 변수는 값이 있을 수도, 없을 수도 있다(nil)  
  
슈뢰딩거의 고양이라고 생각하시면 편할 것 같습니다.  
  

### <span style="color:rgba(0,0,200,0.4)">Wrapping</span>
한번 Optional 변수를 출력해보겠습니다.
```swift
let optionalValue : String? = "Hello, swift"
print(optionalValue)
// 출력 : Optional("Hello, swift")
```

이 경우, 그냥 "Hello, swift" 가 아닌 Optional("Hello, swift")가 출력 됩니다. 이것은 Optional 변수가 nil 인지 값이 있는지 알 수가 없기 때문에 nil 일 경우를 대비해 Optional로 감싸져 있는 것입니다. 이렇게 감싸져 있는 것을 **Wrap** 되어 있다고 합니다.   
  
하지만 위의 경우와 같이 Optional("Hello, swift")는 대부분의 경우 우리가 원하는 출력 값이 아닙니다. 어떻게 Optional 변수를 "Hello, swift" 와 같이 기본 타입으로 사용할 수 있을까요? Optional 변수가 nil 값일 때는 어떤 방법으로 변수를 다룰 수 있을까요?
 
### <span style="color:rgba(0,0,200,0.4)">Forced Unwrapping </span>

Forced Unwrapping 을 통해 Optional 함수를 강제로 Unwrap 할 수 있습니다. 다음의 코드를 살펴보겠습니다.
```swift
let optionalValue:String? = "Hello, Swift"
print(optionalValue!) // ! 를 통한 Forced Unwrapping
// 출력 : Hello, Swift
```
위의 코드처럼 변수뒤에 !를 붙이면 Optional 타입을 Unwrap 할 수 있습니다.  
따라서 출력 값이 Optional("Hello, Swift")가 아닌 Hello, Swift 가 됩니다.  
Forced Unwrapping은 컴퓨터에게 이렇게 말하는 것과 같습니다.
> Optional 변수에 값은 무조건 있으니까 신경쓰지말고 Unwrapping 해!  

하지만 만약 프로그래머의 말과 달리 Optional 변수가 **nil** 값이라면 **에러**가 발생하게 되겠죠.  
따라서 Forced Unwrapping 을 사용할 때는 반드시 Optional 변수에 값이 있다는 것이 **보장** 돼야 합니다. 하지만 모든 상황에서 Optional 변수가 nil 값이 아닌 것을 보장하는 것은 쉽지 않죠. Forced Unwrapping 이 아닌 **다른 방법**도 필요해 보입니다.  
  
Optional 변수를 다루는 다른 방법들에는 4가지가 더 존재합니다.
- Check for nil value
- Optional Binding
- Nil Coalescing Operator
- Optional Chaining  
  
이것들은 [다음 포스팅](https://donggoolosori.github.io/2020/12/13/ios-optional2/){:target="_blank"}에서 좀 더 자세히 다뤄보겠습니다.





