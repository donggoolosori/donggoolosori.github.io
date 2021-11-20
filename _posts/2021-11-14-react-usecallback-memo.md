---
layout: post
title: "[React] useCallback과 React.memo"
subtitle: "React, useCallback, memo"
date: 2021-11-14 22:20:00
author: "dongjune"
header-img: "img/in_post/4.jpg"
catalog: true
tags:
  - React
---
# 🛫 Intro
이번 포스팅에서는 리액트의 최적화 기법 중 `useCallback`과 `memo`에 대해서 알아보도록 하겠습니다.  
리액트 컴포넌트가 리랜더링 될 때마다 그 안에 선언 된 함수들은 매번 **재생성** 되게 됩니다. 이때 `useCallback`을 사용하면 함수의 참조 값을 **유지**해줄 수 있습니다!  
`memo`는 컴포넌트를 메모이제이션 해줍니다. 해당 컴포넌트에게 넘겨주는 `props`가 **동일하면** 같은 랜더링 결과를 리턴하며 리랜더링을 방지해줍니다.    
코드를 살펴보며 제대로 이해하기 전에 우선 리액트가 언제 리랜더링이 일어나는지 확실히 알고 갈 필요가 있습니다! 

# React는 언제 리랜더링 될까?
리액트는 다음의 5가지 경우에서 리랜더링이 일어납니다.
1. state가 변경될 때
2. 새로운 props가 들어올 때
3. 부모 컴포넌트가 리랜더링 될 때
4. shouldComponentUpdate에서 true가 반환될 때
5. forceUpdate가 실행될 때

> 1, 2번의 경우 리액트는 기본적으로 얕은 비교를 통해 값을 비교합니다.  
state 값에 push나 pop 등의 원본을 변경하는 메소드를 사용하지 말아야 하는 이유는 리액트는 얕은 복사를 통해 값의 변경을 판단하기 때문입니다. 원본을 변경한다 해도 참조 값은 그대로이기 때문에 리랜더링은 일어나지 않게 됩니다.  

# 불필요한 리랜더링
부모 컴포넌트의 변경으로 인해 자식 컴포넌트들도 전부 랜더링 되는 경우를 생각해봅시다. 이때 자식 컴포넌트는 아무런 변경이 없어도 리액트는 리랜더링 되는 컴포넌트의 **자식 컴포넌트들까지 전부 리랜더링 시킵니다.**  
아래의 코드는 그 예시입니다. `Parent` 컴포넌트의 `count` 상태가 변경 되어 리랜더링이 일어나면 하위 컴포넌트인 Child 컴포넌트들도 모두 리랜더링이 일어나게 됩니다.
```jsx
import React, { useState } from "react";

export default function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount((prev) => prev + 1);
  };

  return (
    <div>
      <div>{count}</div>
      <button onClick={handleClick}>+</button>
      <Child />
      <Child />
      <Child />
    </div>
  );
}

const Child = () => {
  return <div>Child</div>;
};
```
profiler로 확인해보면 Parent와 3개의 Child 컴포넌트 모두 리랜더링 되는 것을 볼 수 있습니다. 렌더링 시간은 총 3.1ms 만큼 걸리네요.
<img width="962" alt="스크린샷 2021-11-21 오전 1 39 14" src="https://user-images.githubusercontent.com/53213397/142734131-32c883fe-c0f9-43f3-86ee-a2b0e98f7d89.png">

# React.memo로 컴포넌트를 메모이제이션하자!
`React.memo`를 사용하여 자식 컴포넌트들의 불필요한 리랜더링을 막을 수 있습니다. `React.memo`로 감싸진 컴포넌트는 들어오는 `props`의 변경이 없으면 리랜더링이 일어나지 않게 됩니다. 이때 주의할 점은 `React.memo`는 `props`에만 영향을 준다는 사실입니다. `useContext`나 `useState`에서 상태 값의 변경이 일어나면 `React.memo`로 메모이제이션 한 컴포넌트도 여전히 리랜더링이 일어나게 됩니다.  
또 한가지 주의할 점은 `React.memo`를 사용하는 컴포넌트는 동일한 `props`일때 동일한 랜더링 결과를 리턴해야 합니다.  
이제 위의 코드에서 `Child` 컴포넌트를 `React.memo`로 감싸보겠습니다.  
```jsx
const Child = React.memo(() => {
  return <div>Child</div>;
});
```
그 다음 profiler를 사용하여 랜더링을 측정해보겠습니다.  
`Parent` 컴포넌트 하나만 랜더링 되는것이 보이시나요? 랜더링 시간도 0.9ms로 전보다 무려 3배이상 빨라졌습니다!
<img width="960" alt="스크린샷 2021-11-21 오전 1 41 31" src="https://user-images.githubusercontent.com/53213397/142734202-ac0c662c-ae11-4674-8ebc-4418787aa866.png">

# 함수를 props로 넘겨주는 경우
그렇다면 `React.memo`로 메모이제이션 된 하위 컴포넌트들에게 함수를 `props`로 넘겨주는 경우는 어떨까요? 아래의 코드를 살펴보겠습니다.
```jsx
import React, { useState } from "react";

export default function Parent() {
  const [isClicked, setIsClicked] = useState(false);
  const handleOnClick = () => {
    setIsClicked(true);
  };
  return (
    <div>
      <h1>{isClicked ? "true" : "false"}</h1>
      <Child onClick={handleOnClick} />
      <Child onClick={handleOnClick} />
      <Child onClick={handleOnClick} />
    </div>
  );
}

const Child = React.memo(({ onClick }) => {
  return <button onClick={onClick}>Click me!</button>;
});
```

`Child`를 메모이제이션 했으니 `Child`들은 리랜더링이 일어나지 않을까요?
profiler를 확인해보겠습니다.
<img width="959" alt="스크린샷 2021-11-21 오전 1 45 58" src="https://user-images.githubusercontent.com/53213397/142734358-304954a7-0bb3-40dc-806c-53025fba9459.png">

여전히 `Child` 컴포넌트들의 리랜더링이 일어나고 있습니다. 어떻게 된 걸까요?  
그것은 `props`로 넘겨주는 `handleOnClick` 함수가 Parent 컴포넌트의 리랜더링 마다 변경되기 때문입니다!  
**기본적으로 컴포넌트의 리랜더링이 일어나면 그 안에서 선언된 함수들은 모두 재생성 되면서 참조 값이 변하게 됩니다.** 리액트는 참조값을 통해 `props`를 비교하기 때문에 `Child`의 `props`가 변경됐다고 보고 `Child` 컴포넌트들을 리랜더링 시키는 것입니다.

# useCallback으로 함수의 참조값을 유지하자
위의 상황을 `useCallback`으로 해결할 수 있습니다.
사용법은 `useEffect`와 비슷합니다. 첫번째 인자로 메모이제이션을 수행할 `callback` 함수를 넣어주고 두번째 인자로 의존값을 넣어줍니다.  
```jsx
const handleOnClick = useCallback(() => {
    setIsClicked(true);
}, []);
```
위와 같은 경우 의존값을 빈배열로 설정했기 때문에 `handleOnClick` 함수는 `Parent` 컴포넌트가 처음 랜더링 될 때의 참조값을 유지하게 됩니다.  
이제 `Profiler`로 확인해보겠습니다.

<img width="957" alt="스크린샷 2021-11-21 오전 1 47 14" src="https://user-images.githubusercontent.com/53213397/142734396-c48b199c-8b75-40c4-a287-8f29d73e8c00.png">

보시는것과 같이 `Child` 컴포넌트들의 리랜더링이 일어나지 않게 됩니다. 랜더링 시간도 1ms로 2배 빨라졌네요.

# 🚀 Conclusion
이번 포스팅에서 `useCallback`과 `React.memo`에 대해서 알아봤습니다.  
사실 해당 최적화 기법들을 무작정 도입하는 것은 좋은 방법이 아닙니다. `useCallback`과 memo는 추가적인 비교 연산들을 수행하기 때문에 해당 hook을 사용해도 성능 차이가 별로 나지 않거나 최악의 경우 성능이 더 안좋아질 수도 있습니다.  
따라서 이러한 최적화를 도입할 때는 react devtool의 **profiler**를 사용하여 성능 검사와 함께 수행하는 것이 좋습니다.
# Reference
- https://ko.reactjs.org/docs/hooks-reference.html#usecallback