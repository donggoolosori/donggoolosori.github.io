---
layout: post
title: "배열에 비동기 작업을 수행하기"
subtitle: "Asynchronous process in Array"
date: 2021-4-2 21:00:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - JavaScript
---

# <span style="color:rgba(0,0,200,0.7)">Issue</span>
최근에 [daily commit bot](https://github.com/donggoolosori/daily-commit-bot){:target="_blank"}이라는 커밋 알람 텔래그램 봇을 개발하면서, 배열에 ```비동기 작업```을 수행해야 하는 경우가 생겼다.

내가 바랬던 것은 user의 정보가 담긴 배열을 DB에서 불러온 후, 그 user 배열의 모든 user 마다 오늘의 commit을 확인하여 commit 메시지를 보내는 것이었다.  
처음에는 아무생각 없이 아래처럼 ```forEach``` 메소드를 사용했다.
```javascript
// db의 모든 user 정보
const users = data.Items;

users.forEach(async (user)=>{
    ...

    // user의 contribution 갯수 받기
    const totalContributions = await getContributions(query, variables);

    // 오늘 contribution이 없다면 알람을 보낸다.
    if (totalContributions === 0) {
    await bot.sendMessage(chatId, msgPack.getRandomCommitMsg());
    }
})
```
하지만 위의 코드는 정상적으로 작동하지 않았는데, 한참을 씨름한 결과 forEach 메소드가 문제였다는 것을 알게됐다.  

# <span style="color:rgba(0,0,200,0.7)">Solution</span>
위의 코드가 정상적으로 작동하지 않는 이유를 알기 위해서는 먼저 forEach의 동작원리를 이해할 필요가 있다.
### forEach 동작원리 이해하기
아래의 코드는 [MDN](https://developer.mozilla.org/ko/docs/Web/API/NodeList/forEach){:target="_blank"}에서 forEach를 구현한 코드이다.
```javascript
if (window.NodeList && !NodeList.prototype.forEach) {
    NodeList.prototype.forEach = function (callback, thisArg) {
        thisArg = thisArg || window;
        for (var i = 0; i < this.length; i++) {
            callback.call(thisArg, this[i], i, this);
        }
    };
}
```
코드를 보면 forEach는 배열 요소를 돌면서 callback을 호출하지만, 하나의 callback이 끝날 때까지 기다려주지 않는다. 
  
나는 프로젝트에서 ```AWS Lambda```를 사용했기 때문에, 모든 배열요소에 비동기 작업을 끝마치기 전에 Lambda 함수가 종료되어 오류가 발생한 것이었다.  
  
그래서 이 문제는 **forEach**가 아닌 **for** 문을 사용하여 아래처럼 간단하게 해결할 수 있었다.
```javascript
// db의 모든 user 정보
const users = data.Items;

for(let idx=0;idx<users.length;idx++){
    ...

    // user의 contribution 갯수 받기
    const totalContributions = await getContributions(query, variables);

    // 오늘 contribution이 없다면 알람을 보낸다.
    if (totalContributions === 0) {
    await bot.sendMessage(chatId, msgPack.getRandomCommitMsg());
    }
}
```

# <span style="color:rgba(0,0,200,0.7)">Sequence & Parallel </span>
하지만 위의 for문 코드는 users 배열을 돌면서 **순서대로**(sequence) 비동기 작업을 수행하고 있는데, 나의 경우에는 굳이 사용자들에게 순차적으로 메시지를 보낼 **필요가 없다**.  
  
그래서 만약 배열 요소마다 비동기작업을 수행하는 것을 **병렬적**(parallel)으로 수행한다면 성능적으로 훨씬 유리할 것이라 생각했다.  
  
### Promise.all 과 map으로 비동기 병렬처리 구현하기
```javascript
// db의 모든 user 정보
const users = data.Items;

// user마다 알림보내기(contribution이 0일 경우만)
const promises = users.map(async (user) => {
    ...
    
    // user의 contribution 갯수 받기
    const totalContributions = await getContributions(query, variables);

    // 오늘 contribution이 없다면 알람을 보낸다.
    if (totalContributions === 0) {
    await bot.sendMessage(chatId, msgPack.getRandomCommitMsg());
    }
});
await Promise.all(promises);
```
1. ```map```을 사용하여 함수를 병렬적으로 수행한 promise들을 promises 배열에 담기
2. Promise.all 이 pending 상태인 promises들이 모두 resolve 될 때까지 기다림
3. Promise.all 이 resolve 되면 수행완료