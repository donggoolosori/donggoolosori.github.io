---
layout: post
title: "requestAnimationFrame으로 애니메이션 구현하기" 
subtitle: "requestAnimationFrame"
date: 2021-11-21 13:20:00
author: "dongjune"
header-img: "img/in_post/5.jpg"
catalog: true
tags:
  - JavaScript 
---

# 🛫 Intro
`requestAnimationFrame`은 웹 브라우저 상에서 애니메이션을 구동하고자 할 때 흔히 사용되는 Web API입니다.  
이번 포스팅에서는 `requestAnimation`을 사용하는 이유와 기능들을 살펴보고 실습을 통해 실제 사용법까지 살펴보도록 하겠습니다.  
# 🤔 requestAnimationFrame을 사용하는 이유
`requestAnimationFrame` 대신 `setInterval`이나 `setTimeout`을 통해 함수를 반복하여 애니메이션을 구현할 수 있습니다. 그러면 굳이 `requestAnimationFrame`을 사용하는 이유는 뭘까요?  
### 1. 사용자의 모니터 주사율과 애니메이션 동작을 맞춰준다.
우선 `requestAnimationFrame`은 모니터의 주사율에 맞춰서 함수를 실행시켜줍니다. 즉 사용자의 컴퓨터 60FPS라면 1초에 60번씩 실행되는 것입니다.  
하지만 `setTimeout`과 `setInterval`은 사용자의 환경에 맞춰 실행되지 않고 개발자가 설정한 값의 time 간격으로 함수가 실행됩니다. 다음과 같이 60FPS에 맞춰 `setInterval`을 설정한 경우를 생각해봅시다.
```js
setInterval(()=>{
    console.log('hi');
}, 1000/60)
```
144FPS의 주사율을 갖고 있는 사용자의 컴퓨터에서도 60FPS로 동작하게 됩니다. 이는 사용자 컴퓨터 자원을 제대로 활용하지 못하고 낭비하고 있다고 볼 수 있는것이죠. 반대로 코드를 144FPS에 맞췄다고 하더라도 60FPS의 모니터를 사용하는 사용자에게는 60FPS 이상의 프레임으로 볼 수 없기 때문에 이 경우도 불필요한 함수 실행을 하면서 **자원을 낭비**하게 되는것입니다.
### 2. 함수의 실행을 프레임 시작 시점에 동작하도록 해준다.
**setTimeout, setInterval은 프레임을 신경쓰지 않고 동작합니다.** 즉 리페인트가 일어나지 전에 여러번 화면에 그려지는 요소를 변경했다고 하더라도 리페인트 직전의 상태만 실제 화면에 반영되게 됩니다. 이로 인해 **프레임의 손실**을 유발할 여지가 있습니다.  
다음의 그림을 통해 쉽게 이해할 수 있습니다.  

<img width="897" alt="스크린샷 2021-11-21 오후 3 39 18" src="https://user-images.githubusercontent.com/53213397/142752445-7d44dbf3-9524-4a33-a635-0bbd3162e7ed.png">

애니메이션을 그리는 자바스크립트 코드가 프레임 시작 지점이 아닌 중간 지점에서 실행이 되고 결국 실제 프레임에 반영되지 못하여 프레임 유실이 일어났습니다.  
  
하지만 requestAnimationFrame을 사용하면 함수가 프레임 시작 지점에 실행되어 프레임 유실이 일어나지 않도록 돕습니다. 
> 보통 애니메이션을 그리는 함수는 3~4ms 안에 수행되는 것을 권장합니다. 그 이상이 넘어간다면 하나의 requestAnimationFrame 안에서 동작하게 하지 말고 여러개로 나누어 실행시켜야 합니다.
  
# 🔥 requestAnimationFrame 사용하기
<!-- {% include codepen.html username="donggoolosori" hash="XWaQXMJ" title="hello" %} -->





# 🚀 Conclusion
# Reference
- [https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution?hl=ko](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution?hl=ko)
- [https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame)
- [https://darrengwon.tistory.com/641](https://darrengwon.tistory.com/641)