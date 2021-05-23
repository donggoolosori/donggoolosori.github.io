---
layout: post
title: "Node.js Express 로 서버 구축하기"
subtitle: "create http server"
date: 2020-12-06 21:56:00
author: "dongjune"
header-img: "img/in_post/pixel_cafe.gif"
catalog: true
tags:
  - Node.js
---
### <span style="color:rgba(0,0,200,0.6)">기본 Setting</span>
우선 express 패키지를 설치해줍니다.

```bash
$ npm i express
```

이제 package.json을 생성하기 위해 다음 명령어를 수행해줍니다. 

```bash
$ npm init -y
```

루트 폴더에 package.json이 설치 된것을 확인 할 수 있습니다.
  
그리고 nodemon 도 설치해줍니다.

```bash
$ npm i nodemon
```

nodemon 은 소스를 수정했을 때 실시간으로 파일의 변화를 감지하여 자동으로 서버를 재시작 해줍니다.
매번 서버를 껐다 켰다 할 필요가 없으니 개발시 아주 편리합니다.

이제 package.json을 조금 수정해봅시다. 다음과 같이 **main** 부분을 **server.js** 로 바꿔줍니다. 그리고 script를 다음과 같이 수정하여 npm run server를 입력하면 nodemon 으로 server를 실행하도록 설정합니다.  

<img width="700" alt="스크린샷 2020-12-06 오후 10 05 07" src="https://user-images.githubusercontent.com/53213397/101280930-6bd94c80-380f-11eb-8052-414c2fd15458.png">

### <span style="color:rgba(0,0,200,0.6)">express를 통해 서버 구축하기</span>
이제 프로젝트의 루트폴더에 server.js 파일을 생성해주고 아래의 코드를 작성합니다. 코드에 대한 설명은 주석으로 달아놨습니다.

```javascript
// express 불러오기
const express = require("express");

// app 생성
const app = express();
// PORT 번호 기본값 5000으로 설정
const PORT = process.env.PORT || 5000;

// get요청시 "API Running" 을 response 해주기
app.get("/", (req, res) => {
  res.send("API Running");
});

// 첫번째 인자로 PORT 번호
// 두번째 인자로 callback 함수를 통해 server 구축 성공시 console log
app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```

다음과 같이 console log가 뜨며 성공적으로 서버가 구축된 것을 알 수 있습니다.
<img width="702" alt="스크린샷 2020-12-06 오후 10 18 24" src="https://user-images.githubusercontent.com/53213397/101281168-fd958980-3810-11eb-8baa-576b620ae762.png">  

이제 **Postman** 으로 서버에 get 요청을 보내 "API Running" 이라는 응답을 받아봅시다. Postman은 개발한 API를 테스트할 수 있는 유용한 툴입니다.  
[Postman 다운로드 링크](https://www.postman.com/){:target="_blank"}

다음과 같이 localhost 5000 서버에 get 요청을 보내니 서버가 API Running을 응답 하는 것을 확인할 수 있습니다.
<img width="943" alt="스크린샷 2020-12-06 오후 10 08 12" src="https://user-images.githubusercontent.com/53213397/101280965-b1961500-380f-11eb-8084-8f4e0baa5baf.png">