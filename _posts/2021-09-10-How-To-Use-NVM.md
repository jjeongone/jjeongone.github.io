---
layout: post
title: "m1 mac에서 nvm 사용하기"
date: 20210910
published: true
---

## nvm install
brew를 이용하여 nvm을 설치해준다.
```bash
brew install nvm
```
`.zshrc` 파일을 다음과 같이 수정해준다.
```
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```
Terminal을 재시작한 후 다음 커멘드로 nvm이 제대로 설치되었는지 확인해보자!
```bash
nvm --version
``` 
다음 커멘드를 이용하면 현재 설치되어 있는 node 버전들을 확인할 수 있다.
```bash
nvm ls
```
설치 가능한 node 버전에 대해서는 다음과 같이 확인할 수 있다.
```bash
nvm ls -remote
```

<br>
<hr>
<br>

## nvmrc 사용하기

root 디렉토리에 `.nvmrc` 파일을 이용하여 특정 버전의 node를 사용할 수 있다.
```
8.17.0
```
위와같이 단순하게 node의 버전만 작성해주면 된다.
```bash
nvm use
```
`nvm use`를 하면 `Now using node v8.17.0 (npm v6.13.4)` 과 같은 문구와 함께 프로젝트별로 특정 버전의 node를 사용할 수 있다.


<br>
<hr>
<br>

## M1 mac에서 v15 이전 node 설치하기: with *Rosetta2*

> *울지말고 찬찬히 따라해보자!*

M1 mac의 경우 Apple Silicon chip이 장착되어있기 때문에 Intel 환경에서 돌아가는 어플리케이션중에 지원하지 않는 부분이 존재한다. 이럴 때 Rosetta를 이용하면 자동으로 앱을 변환시켜서 Apple Silicon 환경에서도 작동할 수 있게 해준다.

node같은 경우에도 v15 이하를 설치하기 위해서는 Rosetta를 이용해야 한다.

### install
다음과 같은 커멘드로 rosetta를 설치할 수 있다. 설치과정에 나오는 라이선스같은 경우에는 `agree` 해주면 된다.
```bash
softwareupdate --install-rosetta
```

### Rosetta terminal 만들기
Rosetta 환경을 편하게 이용하기 위해 mac에 내장되어 있는 `Terminal`을 복제하여 Rosetta 환경에서 작동하는 새로운 `Terminal`을 만들어보자.

1. Finder에서 Application/Utilities 경로에 들어간다.
2. Terminal을 복제한다
3. 우클릭하여 Get Info(정보 가져오기)를 선택한다
4. Rosetta를 사용하여 열기를 활성화시킨다.
5. 터미널을 켜서 `arch` 를 쳐보면 `i386`을 출력한다.(기존에는 `arm64`)

### node install
Rosetta 세팅을 마쳤으면, 만들어둔 Rosetta terminal에서 `nvm install {node_version}` 하여 원하는 버전의 node를 설치할 수 있다.
