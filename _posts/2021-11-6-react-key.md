---
layout: post
title: "[React] 컴포넌트 리스트와 key 속성"
subtitle: "React, key"
date: 2021-11-6 22:10:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - React
---
## 🛫 Intro
React에서 여러개의 컴포넌트를 렌더링 하고 싶을 때 `map`을 사용할 수 있습니다.

```jsx
const players = ['messi', 'ronaldo', 'son'];

const playerList = numbers.map((player)=> 
	<li>{player}</li>
);
```

하지만 이대로 사용한다면 `li` 요소마다 `key` 값이 필요하다는 에러를 마주치게 됩니다.

이때는 다음과 같이 `index`를 `key` 값으로 사용하여 간단하게 해결할 수 있습니다.

```jsx
const playerList = numbers.map((player, idx)=> 
	<li key={idx}>{player}</li>
);
```

**하지만** 이 방법은 리액트의 성능에 부정적 영향을 줄 수 있습니다. 그 이유는 다음 섹션에서 살펴보죠.

## ❗️Key 값을 index로 설정하지 말아야 하는 이유

key 값은 React가 DOM 엘리먼트들을 식별할 수 있도록 돕습니다. 따라서 key 값이 이전과 같다면 리액트는 해당 엘리먼트를 같은 엘리먼트로 인식합니다. 

그런데 이때 key 값이 index이면 어떤 일이 일어날까요? 기존 `li` 리스트의 앞에 새로운 `li` 요소를 추가해봅시다.

```jsx
// before
<ul>
  <li key={0}>Messi</li>
  <li key={1}>Ronaldo</li>
</ul>

// after
<ul>
  <li key={0}>Son</li>
  <li key={1}>Messi</li>
  <li key={2}>Ronaldo</li>
</ul>
```

리액트는 `key` 값을 통해 컴포넌트들을 식별한다고 했죠.

따라서 모든 `li` 들이 바뀌었다고 인식하게 되며 모든 `li` 들이 리랜더링 되는 현상이 나타나게 됩니다.

우리는 단지 하나의 `li` 만 추가했을 뿐인데 전체 `li` 가 변경 되는 것이 비효율적으로 보이네요. 🤔

그렇다면 다음과 같이 `key` 값을 그 요소의 고유한 정보로 설정해보겠습니다.

```jsx
// before
<ul>
  <li key='messi'>Messi</li>
  <li key='ronaldo'>Ronaldo</li>
</ul>

// after
<ul>
  <li key='son'>Son</li>
  <li key='messi'>Messi</li>
  <li key='ronaldo'>Ronaldo</li>
</ul>
```

앞서 언급한 것 처럼 `key` 값이 같으면 같은 엘리먼트로 인식하게 됩니다. 이제 리액트는 `key`가 son 인 `li`만 추가됐다고 인식하게 되고 나머지 `li`들은 리랜더링이 일어나지 않게 됩니다. 

## 🚀 결론

컴포넌트 리스트가 static 하다면 index를 key 값으로 설정해도 무관합니다. 

하지만 컴포넌트 리스트에 추가, 삽입 등의 변경이 자주 일어난다면 key 값을 index로 설정하는 것은 피해야 합니다.  

## Reference

- [https://ko.reactjs.org/docs/lists-and-keys.html](https://ko.reactjs.org/docs/lists-and-keys.html)
- [https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)