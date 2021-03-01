---
layout: post
title: "[React] React 에서 proxy 설정이 먹히지 않을 때"
subtitle: "React proxy error"
date: 2021-3-1 21:53:00
author: "dongjune"
header-img: "img/in_post/3.jpg"
catalog: true
tags: - React
  - TypeScript
---

stackoverflow에서 검색결과 cache문제인것으로 보인다고 한다. cache 문제를 해결하기 위해 다음의 과정을 수행했다.

1. 다음 명령어를 통해 package-lock.json 과 node_modules 폴더를 삭제해준다

```bash
$ rm -r package-lock.json node_modules
```

2. 다시 설치해준다

```bash
$ npm install
```

**하지만 나의 경우에는 위의 단계를 수행해주어도 오류가 해결되지 않았다.**

cache 문제라고 하였으니 한번 axios 패키지를 재설치해봤다.

```bash
$ npm i axios
```

**그래도 오류가 해결되지 않았다.**

그래서 결국 다음과 같이 **axios instance**를 따로 만들어 base url을 직접 설정하였다.

다음과 같이 axios.ts 파일에서 axios instacance를 생성해주고 export 해주었다.

### axios.ts

```tsx
import axios from 'axios';

const instance = axios.create({
  baseURL: 'https://devconnector-dj.herokuapp.com',
  headers: {
    'Content-Type': 'application/json',
  },
});

export default instance;
```

오히려 이렇게 instance를 직접 만들어주는게 따로 option 값을 설정해주지 않아도 되고, 더 편리한 것 같기도 하다. 다음부터는 이 방법을 적극 사용해보자.