---
layout: post
title: "[Clean Code] Chapter 10. Classes"
date: 20220929
tag: [Book, SD]
published: true
---
<!-- *이 문서는 2022년도 가을학기 포스텍 박성우 교수님의 소프트웨어설계 수업을 들으며 Clean Code를 요약한 문서입니다. `Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다. 오타나 지적은 항상 환영합니다.* 😄

<hr> -->

## Class Organization
Standard Java convention에 따르면, class는 다음과 같은 순서로 구성된다.

- list of **variables**: public static constant가 먼저 선언되고 private static variable이 선언된다
- public functions
- private utilities

### Encapsulation
우리는 보통 encapsulation을 따르려고 하지만, 간혹 variable이나 utility function에 접근하기 위해 protected를 사용하기도 한다. 물론 간편하지만, 코드의 유지보수를 위해 encapsulation을 파괴하는 행위는 최후의 수단으로 남겨둬야 한다.

## Classes Should Be Small!

## Organize for Change

## Thoughts

<hr>

## Reference
- Clean Code