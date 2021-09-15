---
layout: post
title: "m1 mac에서 pintos 개발환경 설정하기"
date: 20210915
published: true
---

## VMware를 사용할 수 없다?

포스팅을 작성하는 현 시점에서 m1 mac에서 실행되는 VMware가 존재하지 않아, VM 환경이 아닌 docker를 통해 Ubuntu 환경을 설정해야 하는 상황이다.

<br>

## Docker에서 Ubuntu 설치하기

Docker같은 경우에는 공식 홈페이지에 m1 mac에서 실행되는 docker가 존재하기 때문에 뚝딱 설치하면 된다.

[docker hub](https://hub.docker.com/)에서 Ubuntu를 검색하면 어느 버전을 다운받을 수 있는지 확인할 수 있다. 혹은 다음 커멘드를 통해 설치 가능한 버전을 알 수 있다.
```bash
docker search ubuntu
```

학교에서 제공한 pintos repo에서는 `Ubuntu 16.04`가 명시되어 있기 때문에 Ubuntu 16.04에 대한 image를 설치한다.
```bash
docker pull --platform linux/amd64 ubuntu:16.04
```

다음 명령어로 image가 잘 받아졌는지 확인할 수 있다.
```bash
docker images
```

이제 imgae를 실행하여 container를 만들어보자
```bash
docker run --platform linux/amd64 -it -d --name pintos ubuntu 
```

실행중인 container 목록은 다음과 같이 확인할 수 있다.
```bash
docker ps
```

다음 커멘드를 이용하여 터미널을 사용할 수 있다.
```bash
docker attach pintos
```

<br>

## VSCode로 docker container에 접속하기

VSCode의 `Remote-Containers` extension을 설치해준다.

VSCode 실행 후 F1 키를 눌러 `Remote-Containers: Attach to Running Container...`를 선택해주면 실행중인 docker container 목록이 나타나고, 그 중 pintos를 선택하면 VSCode를 이용하여 보다 편하게 작업할 수 있다.
