---
layout: post
title: "이벤트의 버블링(bubbling)과 캡쳐링(capturing)"
subtitle: "event, bubbling, capturing"
date: 2021-10-17 15:10:00
author: "dongjune"
header-img: "img/in_post/1.jpg"
catalog: true
tags:
  - JavaScript
---

프론트엔드를 개발하면 정말 많은 상황에서 이벤트를 다루게 됩니다. 

그런데 정작 그 이벤트가 어떤 방식으로 동작하는지에 대해 확실히 알지 못하다 보니 이벤트를 의도대로 동작하게 만들기가 어렵더라구요. 

그래서 오늘은 이벤트가 어떻게 동작하는지 확실히 이해해보도록 하겠습니다.

이벤트가 발생하면 캡쳐링과 버블링이 일어나게 되는데요. 이게 도대체 무엇인지 알기 전에 우선 웹 페이지 상의 버튼을 클릭하는 상황을 살펴봅시다.

## 🤔 버튼을 클릭하면 무슨 일이 일어날까?

먼저 웹의 버튼 엘리먼트를 클릭하는 상황에서 어떤 일이 일어나는지 살펴보도록 합시다. 

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="abydXmb" data-user="donggoolosori" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/donggoolosori/pen/abydXmb">
  event-bubbling</a> by Dongjune Kim (<a href="https://codepen.io/donggoolosori">@donggoolosori</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


버튼을 직접 클릭해보면 button → blue box → red box 순서로 이벤트가 발생하는게 보이시나요?

저는 버튼만 클릭했을 뿐인데 왜 상위 엘리먼트들의 이벤트가 모두 실행될까요?

✅ 바로 앞서 말한 버블링 때문입니다!

## 🧼 버블링

어떤 요소에 이벤트가 발생하면 해당 요소의 이벤트 핸들러가 실행됩니다. 이어서 부모 요소의 이벤트 핸들러가 실행됩니다. 이 과정은 최상위 요소에 도달할 때까지 반복되게 됩니다. 

> 부모 요소들로 이벤트가 계속 올라가는 모습이 마치 물 속의 거품이 위로 올라가는 것과 비슷해서 버블링이라는 이름을 지은게 아닐까 추측해봅니다. 🧐
> 

그럼 위에서 버튼을 눌렀을 때의 동작들이 이해가 됩니다. button을 클릭하면 button의 이벤트 핸들러가 동작되고 부모 요소들로 이벤트가 차례대로 전파되는 것이죠!

## 🎣 캡쳐링

버블링을 이해했으니 캡쳐링을 이해하는데는 문제가 없습니다. 

버블링과는 반대로 부모요소부터 자식요소로 이벤트가 전파되는게 캡쳐링입니다!

캡쳐링에 대해 좀 더 자세히 알아보기 전에 이벤트의 흐름을 알 필요가 있습니다. 표준 [DOM Event]([https://www.w3.org/TR/DOM-Level-3-Events/](https://www.w3.org/TR/DOM-Level-3-Events/))에서 정의한 이벤트 흐름은 다음의 3가지 단계를 따릅니다.

1. 이벤트 캡쳐링 단계 : 최상위 요소에서 event target까지 하위 요소로 이벤트가 전파되는 단계
2. target 단계 : 이벤트가 실제 타겟 요소에 전파되는 단계
3. 이벤트 버블링 단계 : 이벤트가 target에서 최상위 요소까지 상위 요소로 전파되는 단계

<img width="875" alt="스크린샷_2021-10-17_오후_2 49 44" src="https://user-images.githubusercontent.com/53213397/137614412-c471731a-96f2-49d1-b1cf-1038fedf0660.png">


> 실제 이벤트가 발생한 event target으로 상위 요소로부터 이벤트가 내려오는 게 마치 target이 이벤트를 잡는 과정처럼 느껴지네요. 그래서 캡쳐링이라는 이름을 붙인게 아닐까 생각해봅니다. 🧐
> 

위에서 살펴본것과 같이 이벤트 캡쳐링은 버블링보다 먼저 일어나는 단계입니다. 하지만 캡쳐링 단계에서는 이벤트 핸들러가 동작하지 않습니다. 캡쳐링 단계는 이벤트 타겟을 찾아가는 단계로 보시면 될 것 같습니다.

그럼 캡쳐링 단계에서 이벤트를 잡아 핸들러를 실행할 수는 없을까요? 

✅ 가능합니다!

아래와 같이 addEventListener 메소드의 3번째 인자에 capture:true (또는 그냥 true) 옵션을 설정하면 됩니다.

```jsx
redBox.addEventListener('click',(e)=>{
  alert('red box click event');
},{capture:true})

// 또는

redBox.addEventListener('click',(e)=>{
  alert('red box click event');
}, true)
```

그럼 redBox에 캡쳐링에서 동작하는 이벤트 핸들러를 추가해봅시다

<p class="codepen" data-height="300" data-default-tab="html" data-slug-hash="wvqMNXx" data-user="donggoolosori" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/donggoolosori/pen/wvqMNXx">
  event-capturing</a> by Dongjune Kim (<a href="https://codepen.io/donggoolosori">@donggoolosori</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

자 redbox의 캡쳐링 이벤트가 먼저 실행되고 button → blue box → red box 순으로 이벤트가 버블링 되는 것이 보이시죠? 저희가 위에서 배운 내용과 일치하네요! 

캡쳐링이 먼저 일어나기 때문에 캡쳐링 이벤트 핸들러가 먼저 실행되는 것을 확인할 수 있습니다.

다음 포스팅에서는 이벤트의 전파를 막는 stopPropagtion과 stopImmediatePropagation 메소드에 대해서 알아보도록 하겠습니다.

{% include codepen.html hash="QWvyPqB" title="hello" %}