---
layout: post
title: 'VirtualBox에서 SSH 네트워크 설정'
subtitle: 'virtual box, ssh, network'
date: 2021-7-20 22:10:00
author: 'dongjune'
header-img: 'img/in_post/3.jpg'
catalog: true
tags:
  - Server
---

## 🌱네트워크 설정

- 어댑터를 NAT으로 설정한 후 포트 포워딩 클릭합니다.

  ![_2021-07-20__9 34 10](https://user-images.githubusercontent.com/53213397/126329342-8841bf01-4394-436c-82c4-2315a29f3938.png)

- 호스트 포트와 게스트 포트를 설정합니다. 이때 IP 주소를 설정하지 않으면 기본적으로 localhost로 설정 됩니다. 가상머신의 22번 포트와 내 컴퓨터의 22번 포트를 연결하는 과정입니다.

  ![_2021-07-20__9 35 13](https://user-images.githubusercontent.com/53213397/126329355-94f5cb3c-4c59-4470-8858-d47cdd6514b7.png)

- ssh [userid]@[ip] 명령어를 통해 ssh에 접속합니다.

  ![_2021-07-20__9 56 15](https://user-images.githubusercontent.com/53213397/126329376-aa33cf29-8478-4bd6-ba28-ded0009818a2.png)
