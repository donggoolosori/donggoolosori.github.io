---
layout: post
title: "[React] react router dom 사용하기"
subtitle: "React, Router"
date: 2021-11-6 22:20:00
author: "dongjune"
header-img: "img/in_post/3.jpg"
catalog: true
tags:
  - React
---

## 🛫 Intro
여러 페이지로 나뉘는 SPA 프로젝트를 개발한다면 `react-router` 가 필요합니다.
오늘은 웹에서 사용하는 `react-router-dom` 패키지에 대해 알아보겠습니다.
다음과 같이 설치 할 수 있습니다.

```bash
yarn add react-router-dom
```
or
```bash
npm install react-router-dom
```

## 🧀 사용 모듈

```jsx
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
```

### `BrowserRouter`
- history api를 사용하여 URL과 UI를 동기화 해줍니다.
- 이름이 너무 길기 때문에 개발의 편의성을 위해 `as` 를 사용해 Router라는 이름으로 사용합니다.

### `Route`

- path를 설정하여 해당 path에 따라 컴포넌트를 렌더링 해줍니다.

### `Link`

- html의 a태그와 비슷한 용도입니다. to 속성에 설정 된 링크로 이동하며 기록이 history 스택에 저장됩니다.

### `Switch`

- 자식 컴포넌트에 `Route` 나 `Redirect`를 배치하여 매치되는 첫 번째 요소를 렌더링 해줍니다. 매칭 되는 단 하나의 요소만 렌더링 해준다는 것에 주의합니다.

## 🚀 사용 예시

```jsx
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
import { Home, About } from 'pages';

const App = () => {
  return(
    <div>
      <Router>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Router>
    <div/>
  )
}

export default App;
```

### exact path
첫번 째 Route의 경우 `path` 앞에 `exact` 라는 속성을 사용하고 있습니다. 

이렇게 설정해놓지 않으면 `/about` 경로에 들어갔을 때 `/about` 에도 `/` 가 존재하기 때문에 두 컴포넌트가 모두 렌더링 되게 됩니다. `exact` 를 사용해서 그 경로에 정확히 일치하는 컴포넌트만 렌더링 하도록 설정할 수 있습니다.

### Switch 사용하기
아래의 경우에서 `/about/foo` 경로에 접근하게 되면 2번째와 3번째 라우트가 모두 렌더링 됩니다.

```jsx
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
import { Home, About } from 'pages';

const App = () => {
  return (
    <div>
      <Router>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
        <Route path="/about:name" component={About}/>
      </Router>
    <div/>
  )
}

export default App;
```

앞서 설명한 것과 같이 2번째와 3번째 route의 path 모두 `/about`을 포함하고 있기 때문에 나타나는 현상입니다. 모든 Route 별로 exact 속성을 추가하는 방법도 있겠지만 `Switch` 컴포넌트를 사용하여 해결할 수도 있습니다.  

아래와 같이 `Switch`를 사용해봅시다.  

```jsx
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
import { Home, About } from 'pages';

const App = () => {
  return (
    <div>
      <Router>
        <Switch>
          <Route exact path="/" component={Home}/>
          <Route path="/about:name" component={About}/>
          <Route path="/about" component={About}/>
          <Route component={Home}/>
        <Switch>
      </Router>
    <div/>
  )
}

export default App;
```

`Switch` 컴포넌트를 사용하면 자식 컴포넌트 중 path가 일치하는 하나의 컴포넌트만 렌더링 해줍니다.  

`Switch`는 자식 `Route`를 위에서 부터 순차적으로 탐색하기 때문에 Route의 순서를 주의해서 배치해야 합니다.  
예를 들어 `/about:name` path를 갖는 Route 보다 `/about` Route가 위에 있으면`/about/foo` 에 접근해도 항상 `/about` Route의 컴포넌트만 렌더링 될 것입니다.  
가장 마지막 Route에는 path 속성이 없는데 이 Route가 default Route 역할을 하게 됩니다. path가 일치하는 Route가 존재하지 않을 때 이 default Route를 렌더링 해줍니다.
