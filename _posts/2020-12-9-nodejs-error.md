---
layout: post
title: "[Node.js] address already in use 에러 해결하기"
subtitle: "macOs, Linux 해결방법"
date: 2020-12-09 18:26:00
author: "dongjune"
header-img: "img/in_post/pixel_cafe.gif"
catalog: true
tags:
  - Node.js
--- 

npm run 명령어를 사용하여 nodejs를 실행하려 할때 다음과 같은 오류가 발생할 때가 있습니다.
<img width="700" alt="스크린샷 2020-12-09 오후 6 06 54" src="https://user-images.githubusercontent.com/53213397/101611562-02468180-3a4d-11eb-81d7-c4b0ac508a07.png">

포트가 이미 다른 프로세서에서 사용중이기 때문에 발생하는 오류입니다.

현재는 5000번 포트가 사용중이라서 에러가 났습니다.

**lsof** 명령어를 통해 터미널에서 활성화 된 프로세스들의 리스트를 확인할 수 있습니다.

-i 를 통해 특정 포트를 사용하는 프로세스를 보여줍니다.

```bash
$ lsof -i TCP:5000
```

다음의 출력결과가 나옵니다. 5000번 포트를 사용중인 것을 알 수 있습니다.
<img width="700" alt="스크린샷 2020-12-09 오후 6 10 34" src="https://user-images.githubusercontent.com/53213397/101611579-05da0880-3a4d-11eb-9963-c47d1340b197.png">

가장 왼쪽에서 두번째가 프로세스의 **PID** 값입니다. 

이제 이 프로세스를 종료해봅시다. 위에서 찾은 PID 값을 붙여 kill -9 으로 삭제해줍니다.

```bash
$ kill -9 68742
```

kill을 해준 후 다시 lsof 명령어로 5000번 포트를 사용하는 프로세스를 확인해봅시다.

다음과 같이 성공적으로 프로세스를 종료 한 것을 확인할 수 있습니다.

<img width="700" alt="스크린샷 2020-12-09 오후 6 13 31" src="https://user-images.githubusercontent.com/53213397/101611580-06729f00-3a4d-11eb-8da3-bee9a3e8db59.png">
