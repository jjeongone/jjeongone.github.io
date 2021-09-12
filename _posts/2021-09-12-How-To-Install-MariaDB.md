---
layout: post
title: "MariaDB 설치하기"
date: 20210910
published: true
---

## Install mariaDB
brew를 이용하여 MariaDB를 설치한다.
```bash
brew install mariadb
```
MariaDB를 실행시킨다.
```bash
brew services start mariadb
```
실행중인 service는 다음과 같이 확인할 수 있다.
```bash
brew services list
```
MariaDB를 멈추려면 다음과 같은 명령어를 이용한다.
``` bash
brew services stop mariadb
```

<br>

## MariaDB 설정
MariaDB 설치가 완료되고 실행이 되었으면 초기 비밀번호 설정을 해준다.
```bash
sudo mysql -u root -p mysql
```
여기서 `1234`는 원하는 root 사용자 비밀번호를 입력하면 된다.
```bash
set password for 'root'@'localhost' = PASSWORD('1234');
```
이제 mysql -u root -p 로 접속가능하다.
