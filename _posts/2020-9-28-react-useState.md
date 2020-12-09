---
layout: post
title: "[React] 리액트 Hooks - useState"
subtitle: "Hooks의 useState"
date: 2020-09-28 23:04:00
author: "dongjune"
header-img: "img/in_post/code-bg.jpg"
catalog: true
tags:
  - Web
  - React
---

React 버전 16.8 부터는 **Hooks** 을 이용하여 **Class** 없이 **state** 를 다룰 수 있다.  
우선 기존에 class를 사용하여 state를 다뤘던 방법부터 살펴보자.

# class를 이용하여 state 다루기

```jsx
import React from "react";

class Counter extends React.Component {
  // 생성자
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  render() {
    return (
      <div>
        <div>Count : {this.state.count}</div>
        // Plus button, count가 1증가
        <button
          onClick={() =>
            this.setState((current) => ({ count: current.count + 1 }))
          }
        >
          Plus
        </button>
        // Minus button, count가 1감소
        <button
          onClick={() =>
            this.setState((current) => ({ count: current.count - 1 }))
          }
        >
          Minus
        </button>
      </div>
    );
  }
}

export default Counter;
```

위 처럼 class를 사용해 state를 다룰 수 있지만 코드가 다소 지저분하다. Hooks를 사용하면 보다 간결하게 state를 다룰 수 있다.

# Hooks의 useState를 사용하여 state 다루기

```jsx
// useState 함수 import하기
import React, { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <div>Count: {count}</div>
      <button onClick={() => setCount(count + 1)}>Plus</button>
      <button onClick={() => setCount(count - 1)}>Minus</button>
    </div>
  );
};

export default Counter;
```

class를 사용했을 때 보다 훨씬 간결해진 모습이다. 그렇다면 위의 코드에서 useState가 정확히 어떤일을 하는지 확인해보자.

```jsx
const [count, setCount] = useState(0);
```

위의 코드는 JavaScript의 **배열구조분해** 문법이다.  
useState는 **2개의 값을**가진 배열을 반환하는데 **첫번째 값은 state 변수** 이며 **두번째 값은 해당 변수를 갱신할 수 있는 함수** 이다.  
class를 사용했을 때 this.state.count를 통해 state 변수에 접근했던것과는 달리 **useState 함수를 통해 state 변수를 할당하면** 아래의 코드처럼 state 변수에 바로 접근할 수 있다.

```jsx
<div>Count: {count}</div>
```

또한 아래의 코드처럼 state 변수를 갱신 할 수 있다.

```jsx
<button onClick={() => setCount(count + 1)}>Plus</button>
```

여기까지 Hooks를 사용해 state를 다루는 법을 알아보았다. 다음 포스팅에서는 Hooks의 useEffect 함수를 통해 state의 life cycle을 다루는 법을 알아보자.
