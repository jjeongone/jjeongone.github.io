---
layout: post
title: "Jekyll blog 만들기"
---
## 개발환경 세팅
### 1. 루비 설치하기
Jekyll이 Ruby 기반으로 만들어져 있기 때문에 우선 Ruby를 설치해 주어야 한다. ~~Ruby에 대해서 아는 것은 없지만 우선 설치해보는 걸로 하자~~

- 윈도우의 경우 Ruby 홈페이지에서 `WITH DEVKIT`을 설치해 준다.

    > <https://rubyinstaller.org/downloads/> 

- Mac OS의 경우 

    루비 버전 관리도구인 RVM을 설치한다
    ```
    \curl -sSL https://get.rvm.io | bash -s stable
    ```
    RVM을 이용하여 최신버전의 Ruby를 설치해준다
    ```
    rvm install ruby-2.6.6
    ```
    버전확인은 다음과 같이 할 수 있다.
    ```
    ruby -v
    ```

### 2. 패키지 설치
jekyll을 실행하는 데 필요한 패키지를 설치한다. 
```
gem install jekyll bundler
```

### 3. 기본 탬플릿 생성하기
jekyll 블로그를 만들 directory에서 다음과 같은 커멘드를 입력하면, 기본 탬플릿의 jekyll 블로그 파일이 형성된다.
```
jekyll new .
```

### 4. 로컬에서 실행하기
다음과 같은 커멘드로 로컬환경에서 블로그를 실행시킬 수 있다. 로컬환경에서 실행에 성공하면 커멘드라인데 주소가 나타나고, 해당 경로를 통해 블로그가 원하는 대로 작동하는지 테스트할 수 있다.
```
jekyll serve
```
***
## 포스트 작성하기
> **포스트의 기본 구성**
> - file_name
> - layout
> - title
> - tag

***
## 입맛대로 꾸미기
***
## Markdown 기본문법