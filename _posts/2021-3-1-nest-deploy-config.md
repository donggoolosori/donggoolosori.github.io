---
layout: post
title: "[NestJs] Heroku로 NestJs 배포 시 config 값 설정"
subtitle: "NestJs heroku deploy"
date: 2021-3-1 22:00:00
author: "dongjune"
header-img: "img/in_post/4.jpg"
catalog: true
tags:
  - NestJs
  - TypeScript
---
heroku를 통해 NestJS 앱을 배포할 때,

아래처럼 heroku 앱이 **.env에 설정해놓은 MongoDB의 URI를 읽지 못하여** Unable to connect to the database 오류가 발생했다.  

![_2021-02-25__1 27 15](https://user-images.githubusercontent.com/53213397/109500312-6ff8cb80-7ad9-11eb-8e00-5d87402aef36.png)
프로젝트에서 MongoDB의 URI나 Json web token 의 key 같은 config 값들을 모두 .env 파일로 관리하였는데, heroku가 .env 파일을 인지하지 못해서 발생하는 오류로 보였다.  

1시간 동안 이것저것 찾아보다가 결국 heroku에게 직접 config 값들을 설정해주는 방법으로 오류를 해결하였다.

NestJS 앱의 root에서 다음의 명령어를 통해 config를 설정할 수 있다. 

```bash
$ heroku config:set MONGODB_URI="YourMongoURI"
$ heroku config:set JWT_SECRET="mysecret"
```

그 후 배포를 해보니 정상적으로 배포가 완료됐다.  
아래는 정상적으로 배포가 된 사이트이다.  
https://donggoolosori.github.io/devconnect