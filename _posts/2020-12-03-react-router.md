---
layout: post
title: "[React] react-router-dom 사용하기"
subtitle: "BrowserRouter,Route,Link,Switch"
date: 2020-12-03 17:30:00
author: "dongjune"
header-img: "img/in_post/pixel_cafe.gif"
catalog: true
tags:
  - React
---

react-router-dom 은 SPA 구현에 사용되는 React의 package입니다.

#### SPA 란?
**SPA는** Single Page Application 의 약자입니다. 
화면의 header와 footer와 같이, 페이지를 이동해도 변하지 않는 요소들이 있습니다.  
이때 매번 화면을 이동할 때 마다 모든 요소들을 렌더링하는 것은 비효율적 입니다.  
**SPA는** 변하지 않는 요소들은 그대로 두고 변하는 요소들만을 부분적으로 렌더링하는 방식입니다.  

#### react-router-dom 설치
React 프로젝트의 root 폴더로 가서 다음 명령어를 사용해 package를 설치할 수 있습니다.
```bash
$ npm i react-router-dom
```



### <span style="color:rgba(0,0,200,0.4)">BrowserRouter</span>  
react-router-dom 에는 BrowserRouter 와 HashRouter 두 가지의 router가 존재합니다.  
BrowserRouter는 백엔드를 사용해야 할 때, 즉 동적인 페이지에 적합한 Router입니다. 따라서 거의 대부분에 웹 페이지에는 BrowserRouter가 주로 사용됩니다.  
반면에 HashRouter는 백엔드가 필요하지 않은, 즉 정적인 페이지에 사용하기 적합합니다. 
> www.domain.com/#/user  

  
HashRouter의 주소체계는 location.hash에 대응되며 위와 같은 형식입니다.  
HashRouter에서 위 domain을 server에 요청하면 server는 이를 # 이전까지인 www.domain.com 의 응답을 줍니다.  
> www.domain.com/user/1234  


BrowserRouter에서는 location.pathname 을 이용해 값을 얻을 수 있습니다.  
위의 domain을 server에 요청하면 server도 마찬가지로 www.domain.com/user/1234 에 대한 응답을 줍니다.

### <span style="color:rgba(0,0,200,0.4)">Route</span>
요청 받은 pathname 의 Component를 렌더링 해줍니다.
```jsx
<Route path="/" component={Home}/>
```
### <span style="color:rgba(0,0,200,0.4)">Link</span>
link를 생성하여 해당 page 간 이동을 가능하게 해줍니다.  
html의 a 태그와 유사합니다.  
```jsx
<Link to="/register"></Link>
```
### <span style="color:rgba(0,0,200,0.4)">Switch</span>
Route 들 간 path의 충돌을 방지해줍니다.  
Switch component 내부에 Route 들을 넣어주면 요청 된 path와 일치하는 첫 Route 를 렌더링 해줍니다.  
따라서 Switch component 안의 Route 들의 순서가 중요할 수도 있습니다.  
```jsx
<Switch>
    <Route exact path="/" component={Home}/>
    <Route exact path="/register" component={Register} />
    <Route exact path="/login" component={Login} />
</Switch>
```
여기서 주의해야 하는 것이 바로 Route 의 **exact** 속성입니다.  
만약 exact 속성을 넣어주지 않으면 모든 요청에 Home component가 렌더링 됩니다. 왜냐하면 "/" 주소는 모든 주소에 매칭되기 때문입니다. 따라서 exact 속성을 넣어서 주소가 정확히 일치할 때에만 렌더링 하도록 해야합니다.

### <span style="color:rgba(0,0,200,0.4)">react-router-dom을 사용한 App 만들어보기</span>
이제 위에서 설명한 내용을 토대로 router를 사용한 간단한 App을 만들어봅시다.   
#### 1. react app 만들기 및 react-router-dom 설치
npx 명령어를 통해 react app을 생성합니다. 
```bash
$ npx create-react-app name-of-app
```
그 후 react 앱의 루트 폴더에서 react-router-dom을 설치합니다.
```bash
$ npm i react-router-dom
```
#### 2. component import 하기
이제 필요한 component 들을 import 해줍니다.
BrowserRouter, Link, Route, Switch 를 사용해봅니다. BrowserRouter의 이름이 너무 길어서 BrowserRouter as Router 로 import 하여 사용을 간단하게 해줍니다.

**App.js**
```jsx
import React from "react";
import {BrowserRouter as Router, Route, Link, Switch} from "react-router-dom";

const App = () => {
  return <div className="App">Hello, React Router!</div>;
};

export default App;
```
<img width="553" alt="router1" src="https://user-images.githubusercontent.com/53213397/117609882-c5b5b800-b19b-11eb-8742-00cedf6204c8.png">

#### 3. Router 안에 Link 생성
**App.js**
```jsx
import React from "react";
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
import "./styles.css";

const App = () => {
  return <div className="App">
    Hello, React Router!
    <Router>
      <ul>
        <li>
          <Link to='/'>Home</Link>
        </li>
        <li>
          <Link to='/register'>Register</Link>
        </li>
        <li>
          <Link to='/rogin'>Rogin</Link>
        </li>
      </ul>
      </Router>
    </div>;
};

export default App;
```
<img width="549" alt="router2" src="https://user-images.githubusercontent.com/53213397/117609886-c6e6e500-b19b-11eb-9c98-76ba50855859.png">

위의 사진처럼 Register Link를 눌렀을 때 주소창에 /register가 요청되는 것을 확인 할 수 있습니다. 이제 저 경로상에 component 들을 할당 해 줄 차례입니다.

#### 4. Switch 안에 Route 생성
우선 Register, Login 컴포넌트를 생성해줍니다.  

**Register.js**  
<img width="529" alt="router3" src="https://user-images.githubusercontent.com/53213397/117609887-c77f7b80-b19b-11eb-9bcf-b587dbfbbc07.png">

**Login.js**  
<img width="534" alt="router4" src="https://user-images.githubusercontent.com/53213397/117609891-c8181200-b19b-11eb-90b3-9b8b50bc281d.png">

그 후 App.js 에서 이 2개의 컴포넌트를 import 해줍니다.  
**App.js**
<img width="757" alt="router5" src="https://user-images.githubusercontent.com/53213397/117609893-c8b0a880-b19b-11eb-9db6-43d1fdee7595.png">


이제 Switch 컴포넌트 안에 Route를 만들어줍니다.  
<img width="473" alt="router6" src="https://user-images.githubusercontent.com/53213397/117609895-c9493f00-b19b-11eb-9fdb-3084fa211f8a.png">

  
완성된 코드입니다.  

**App.js**
```jsx
import React from "react";
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";
import "./styles.css";
import Register from "./Register";
import Login from "./Login";

const App = () => {
  return (
    <div className="App">
      Hello, React Router!
      <Router>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/register">Register</Link>
          </li>
          <li>
            <Link to="/login">Login</Link>
          </li>
        </ul>
        <Switch>
          <Route exact path="/register" component={Register} />
          <Route exact path="/login" component={Login} />
        </Switch>
      </Router>
    </div>
  );
};

export default App;

```
웹에서의 모습입니다.
Register 링크를 누르면 Link 컴포넌트에 의해 주소창에 /register가 나타나는 것을 볼 수 있습니다.  
그리고 Switch와 Route 컴포넌트에 의해 Register component가 렌더링 되는 것을 확인할 수 있습니다.
<img width="488" alt="router7" src="https://user-images.githubusercontent.com/53213397/117609896-c9e1d580-b19b-11eb-8e5e-995fd1d86aa8.png">

Login 링크를 누르면 마찬가지로 주소창의 주소가 /login으로 바뀌고 Login 컴포넌트가 렌더링 됩니다.
<img width="488" alt="router8" src="https://user-images.githubusercontent.com/53213397/117609897-c9e1d580-b19b-11eb-9850-72c1254e349d.png">


