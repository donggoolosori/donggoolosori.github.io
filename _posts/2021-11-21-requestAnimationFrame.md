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
  
하지만 `requestAnimationFrame`을 사용하면 함수가 프레임 시작 지점에 실행되어 프레임 유실이 일어나지 않도록 돕습니다. 
> 보통 애니메이션을 그리는 함수는 3~4ms 안에 수행되는 것을 권장합니다. 그 이상이 넘어간다면 하나의 requestAnimationFrame 안에서 동작하게 하지 말고 여러개로 나누어 실행시켜야 합니다.
  
# 🔥 requestAnimationFrame 사용하기
`requestAnimationFrame`을 사용하여 간단한 카운터를 만들어봤습니다.  
+, - 버튼을 꾹 누르고 있으면 숫자가 연속적으로 증가, 감소합니다. 이제 코드를 살펴보겠습니다.  

`requestAnimationFrame`은 한번만 실행되는 함수입니다. 따라서 다음과 같이 재귀적으로 실행되도록 합니다. 그리고 리턴값으로 `requestAnimationFrame`의 `id` 값을 받습니다. 해당 `id` 값은 `cancleAnimationFrame`에 인자 값으로 넘겨주어 애니메이션을 중단시키기 위해 사용합니다.
```js
const plusCount = () => {
  countDiv.innerHTML = ++count;
  rafId = requestAnimationFrame(plusCount);
};
```
이제 `plusCount` 함수를 `mousedown` 이벤트에 등록합니다. 그리고 mouseup 이벤트에서 저장했던 `rafId` 값을 넘겨주어 `cancleAnimationFrame` 을 실행시켜줍니다. 
```js
plusBtn.addEventListener('mousedown', () => {
  plusCount();
});

plusBtn.addEventListener('mouseup', () => {
  cancelAnimationFrame(rafId);
});
```

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="XWaQXMJ" data-user="donggoolosori" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/donggoolosori/pen/XWaQXMJ">
  Untitled</a> by Dongjune Kim (<a href="https://codepen.io/donggoolosori">@donggoolosori</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>





# 🚀 Conclusion
이번 포스팅에서 간단하게 `requestAnimationFrame`과 그 사용법에 대해 알아봤습니다.  
웹 프론트엔드 개발을 할 때 애니메이션을 다뤄야하는 경우가 많습니다. 그럴 때 `requestAnimationFrame`을 사용하면 프레임 손실을 방지할 수 있고 사용자의 환경에 맞게 애니메이션을 최적화 할 수 있습니다.  
저도 앞으로 애니메이션을 구현할 때 해당 API를 적극적으로 사용해봐야겠습니다.  
# Reference
- [https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution?hl=ko](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution?hl=ko)
- [https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame)
- [https://darrengwon.tistory.com/641](https://darrengwon.tistory.com/641)