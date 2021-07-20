---
layout: post
title: '[Linux] 리눅스의 권한(Permission) - chmod, chown'
subtitle: 'linux, permission, chmod, chown'
date: 2021-7-20 23:20:00
author: 'dongjune'
header-img: 'img/in_post/3.jpg'
catalog: true
tags:
  - Server
---

# ⭐️Linux Permission

linux는 여러 사용자가 사용하는 멀티유저 시스템이기 때문에 파일시스템에 Permission이라는 기능이 존재합니다.

## 🐝파일의 정보

다음과 같이 ls -al 명령어를 통해 파일들의 상세한 정보를 확인할 수 있습니다.

![_2021-07-20__10 41 26](https://user-images.githubusercontent.com/53213397/126340286-f948763c-9c00-49ed-8eb5-7e81b701da7e.png)

위 사진에서 가장 위에 있는 파일 정보를 해석해봅시다.

## drwxr-xr-x 17 dongjune dongjune 4096 7월 20 22:40 .

### 🌱d : 파일 타입

d는 파일의 type 정보입니다. d는 directory, l은 링크파일, -은 일반 파일 등을 의미합니다.

### 🌱rwxr-xr-x : Permission 정보

그리고 r,w,x는 다음을 의미합니다.

- 읽기 (r): 파일의 읽기 권한, 4
- 쓰기 (w): 파일의 쓰기 권한, 2
- 실행 (x): 파일의 실행 권한, 1

permission 정보를 3등분 해보면 rwx r-x r-x 이고 각각 소유자, 그룹, 공개 사용자에 대한 permission 정보입니다.

r,w,x,를 숫자로 변환하여 더해주면 permission 숫자 값을 얻을 수 있습니다.

rwx = 4 + 2 + 1 = 7

### 🌱17 : 링크 수

이 숫자는 해당 파일이 링크 된 수입니다. 링크는 윈도우의 바로가기와 같습니다.

### 🌱dongjune : 소유자

해당 파일의 소유자 이름입니다.

### 🌱dongjune : 소유 그룹

해당 파일의 소유 그룹입니다. 기본 값은 소유자가 속한 그룹입니다.

### 🌱4096 : 용량

파일의 용량을 뜻합니다.

### 🌱7월 20 22:40 : 생성 날짜

해당 파일의 생성 날짜를 의미합니다.

### 🌱. : 파일명

파일명입니다.

## 🐝Permission 변경하기

### 🌱chmod

chmod 명령어를 통해 permission 값을 변경할 수 있습니다.

다음과 같이 test 디렉토리를 생성하고 파일의 퍼미션 정보를 확인해봅니다.

![_2021-07-20__11 00 26](https://user-images.githubusercontent.com/53213397/126340294-5a9d52ff-1dfe-4686-9164-c0ffdbe39498.png)

현재 test 디렉토리의 퍼미션 정보는 rwxrwxr-x이고 이를 숫자로 변환하면 (4+2+1)(4+2+1)(4+0+1) 로 775가 됩니다. 이제 이 파일의 퍼미션을 rwxrw-r-x(765)로 변환해보겠습니다.

다음의 명령어를 입력합니다.

```bash
chmod 765 ./test
```

다음과 같이 test 디렉토리의 퍼미션 정보가 rwxrw-r-x로 변환된 것을 확인할 수 있습니다.

![_2021-07-20__11 03 56](https://user-images.githubusercontent.com/53213397/126340299-7f078980-ec6d-473c-aacd-88dbc55808ef.png)

### 🌱chown

chown은 파일의 소유자를 변경하는 명령어입니다. 이름 그대로 change own 기능을 수행합니다.

현재 dongjune이 소유자인 test 파일을 root 소유로 바꿔보겠습니다.

아래의 명령어를 입력합니다. root로 바꿔주는 것이기 때문에 앞에 sudo를 붙여줍니다.

```bash
sudo chown root ./test
```

다음과 같이 파일의 소유자가 root로 변환된 것을 확인할 수 있습니다.

!![_2021-07-20__11 08 43](https://user-images.githubusercontent.com/53213397/126340301-157e9f52-9a33-43d2-b57e-e12a23b8f56a.png)

소유 그룹을 바꾸기 위해서는 소유자 뒤에 .그룹이름을 붙여주면 됩니다.

파일을 root소유자 root 그룹 소유자로 바꿔보겠습니다.

```bash
sudo chown root.root ./test
```

아래와 같이 소유자와 소유그룹 모두 root로 변경된 것을 확인할 수 있습니다.

![_2021-07-20__11 12 21](https://user-images.githubusercontent.com/53213397/126340303-2c91f7c9-a6cd-4811-bfa1-61c1db14f4ea.png)
