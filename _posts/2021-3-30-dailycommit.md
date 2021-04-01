---
layout: post
title: "Serverless framework를 사용하여 AWS Lambda 함수 배포하기"
subtitle: "Aws Lambda, Serverless"
date: 2021-3-30 21:00:00
author: "dongjune"
header-img: "img/in_post/3.jpg"
catalog: true
tags:
  - AWS
---
# Intro
![1](https://media.vlpt.us/images/jeffyoun/post/52df101a-88e2-4152-8f74-579d0abaaae6/serverless-components.gif)
**Serverless Framework** 는 AWS Lambda와 같은 serverless 애플리케이션의 운용, 배포, 환경설정 등을 아주 손쉽게 할 수 있도록 도와줍니다.
오늘은 Serverless를 사용하여 AWS Lambda 함수를 작성하고 이를 배포해보도록 하겠습니다.

# Install
serverless framework를 설치해줍니다.
```bash
$ npm i -g serverless
```
설치 확인을 위해 ```$ sls --version``` 을 입력해봅니다. 
다음과 같이 뜬다면 정상적으로 설치 완료입니다.
<img width="580" alt="스크린샷 2021-04-01 오후 2 00 10" src="https://user-images.githubusercontent.com/53213397/113245807-a735e480-92f2-11eb-94c1-4ef4e4b68a3b.png">

# AWS IAM 설정
serverless를 통해 나의 AWS 계정에 Lambda 함수를 배포하기 위해서는 serverless가 나의 AWS 계정에 대한 권한을 갖고있어야 합니다.  
그러기 위해서 우선 AWS의 액세스 권한을 안전하게 제어할 수 있는 IAM을 생성해줍니다.  
  
[IAM](https://console.aws.amazon.com/iam/home) 링크에 들어가서 사용자 -> 사용자 추가를 선택합니다.
### 1. 사용자 이름을 설정하고 프로그래밍 방식 엑세스를 선택합니다.
<img width="1028" alt="스크린샷 2021-04-01 오후 2 09 59" src="https://user-images.githubusercontent.com/53213397/113246657-6f2fa100-92f4-11eb-873a-d2729085727a.png">

### 2. 기존 정책 직접 연결을 선택 후 AdministratorAccess를 선택합니다.
<img width="1033" alt="스크린샷 2021-04-01 오후 2 11 56" src="https://user-images.githubusercontent.com/53213397/113246673-79519f80-92f4-11eb-9771-8e1a5e8a6217.png">

### 3. 태그는 추가는 그냥 넘어가줍니다.
<img width="1004" alt="스크린샷 2021-04-01 오후 2 12 08" src="https://user-images.githubusercontent.com/53213397/113246686-7c4c9000-92f4-11eb-91f1-abeb14d6bc6f.png">

### 4. 4번도 그냥 넘어갑니다.
<img width="991" alt="스크린샷 2021-04-01 오후 2 12 18" src="https://user-images.githubusercontent.com/53213397/113246693-81a9da80-92f4-11eb-9ebb-bd6e7c93ded4.png">

### 5. 이제 다음과 같이 엑세스 키 ID와 비밀 엑세스 키를 얻을 수 있습니다. 이 ID와 비밀 키를 잘 저장해둡니다.
<img width="1021" alt="스크린샷 2021-04-01 오후 2 12 44" src="https://user-images.githubusercontent.com/53213397/113246703-85d5f800-92f4-11eb-8414-2c2d2a56a530.png">

# serverless 권한 설정
위에서 얻은 AWS 엑세스 ID와 비밀 키를 serverless에 설정해줍니다.
```bash
$ serverless config credentials --provider aws --key {액세스 키 ID} --secret {비밀 액세스 키}
```

# Serverless Template 생성하기
```bash
$ serverless create --help
```
위의 명령어를 통해 template list를 확인할 수 있습니다.
<img width="1151" alt="스크린샷 2021-04-01 오후 2 27 25" src="https://user-images.githubusercontent.com/53213397/113247635-6344de80-92f6-11eb-91bc-d17216bc014f.png">
저희는 nodejs 앱을 만들기 위해 aws-nodejs template을 사용하여 다음의 명령어를 입력합니다.
```bash
$ serverless create --template aws-nodejs --path my-service
```
--template 뒤에는 template 종류, --path 뒤에는 프로젝트 이름을 입력합니다.

다음과 같이 성공적으로 serverless 앱을 생성했습니다.
<img width="676" alt="스크린샷 2021-04-01 오후 2 31 06" src="https://user-images.githubusercontent.com/53213397/113247925-e8c88e80-92f6-11eb-93bc-829c48d19543.png">
<img width="789" alt="스크린샷 2021-04-01 오후 2 32 09" src="https://user-images.githubusercontent.com/53213397/113247983-0d246b00-92f7-11eb-815b-cbe29e5c2e67.png">

# serverless.yml 파일 
이제 생성한 앱을 AWS에 배포하기 전에 serverless.yml 파일을 한번 살펴보겠습니다.
```yml
service: my-service
frameworkVersion: '2'

provider:
  name: aws
  runtime: nodejs14.x
  lambdaHashingVersion: 20201221
  region: ap-northeast-2

functions:
  hello:
    handler: handler.hello
```
우선 region을 **ap-northeast-2**로 설정하여 AWS Lambda가 한국서버에서 실행되도록 설정합니다.  
그리고 저는 nodejs 버전은 14로 설정하기 위해 **runtime: nodejs14.x**로 설정했습니다.  
이 정도면 기본적인 설정을 끝입니다.

# 배포 전 테스트
Lambda 함수를 테스트하기 위해 매번 배포하는 것은 꽤 번거롭기 때문에 다음의 명령어를 통해 함수를 테스트 할 수 있습니다.
```bash
$ serverless invoke local --function hello
```
--function 뒤에는 함수명을 입력해줍니다.  
  
그러면 다음과 같이 함수가 정상적으로 테스트 되는 것을 확인할 수 있습니다.
<img width="699" alt="스크린샷 2021-04-01 오후 2 41 32" src="https://user-images.githubusercontent.com/53213397/113248675-5d4ffd00-92f8-11eb-8469-94e41f8328b8.png">
# 배포
이제 함수를 배포해보도록 하겠습니다.
```bash
$ sls deploy
```
<img width="879" alt="스크린샷 2021-04-01 오후 2 49 37" src="https://user-images.githubusercontent.com/53213397/113249403-93da4780-92f9-11eb-9d64-d5647d530ee2.png">
아래와 같이 AWS 에도 my-service 앱이 성공적으로 등록됐습니다.
<img width="1422" alt="스크린샷 2021-04-01 오후 2 51 22" src="https://user-images.githubusercontent.com/53213397/113249492-bcfad800-92f9-11eb-865c-6d98126b939b.png">

# 마치며
오늘은 AWS Lambda 함수를 serverless framework를 사용하여 간단하게 배포하는 법을 알아봤습니다.  
다음 포스팅에서는 AWS Lambda layer를 사용하여 Lambda 함수의 패키지 모듈을 관리하는 법을 알아보겠습니다.
