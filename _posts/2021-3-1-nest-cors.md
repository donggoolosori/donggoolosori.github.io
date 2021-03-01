---
layout: post
title: "[NestJs] NestJS에서 CORS 해결하기"
subtitle: "NodeJs, Typescript"
date: 2021-3-1 21:40:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - NestJs
  - TypeScript
---
### main.ts에서 아래의 코드 중 한가지 사용
1. app.enableCors() 코드 추가
```jsx
const app = await NestFactory.create(AppModule);
app.enableCors();
await app.listen(3000);
```
 
2. NestFactory.create() 메소드에 cors:true 옵션 추가
```jsx
const app = await NestFactory.create(AppModule, { cors: true });
await app.listen(3000);
```