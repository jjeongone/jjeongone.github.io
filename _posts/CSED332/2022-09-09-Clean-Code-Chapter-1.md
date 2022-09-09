---
layout: post
title: "[Clean Code] Chapter 1. Clean Code"
date: 20220909
tag: [Book, SD]
published: true
---
*`Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다.*

*오타나 지적은 항상 환영합니다.*

## There Will Be Code
우리는 code를 왜 작성하는가. Code는 어떠한 요구나 목적을 달성하기 위한 *언어*이고, 철저하고 엄격하며 정확하다는 특징을 가진다.

## Bad Code
Bad code는 코드를 짜는 순간에는 시간을 단축시키고 고민을 덜 할 수 있지만, 결과적으로 유지보수에 영향을 준다. Bad code에 의해 무언가 지연되는 것을 `wading`이라 표현한다. 아마도 대부분의 사람들은 bad code를 짜는 것을 피하고 싶어할 것이다. 하지만, 현실은 녹록치 않다. 시간적 제약, 주변의 압박, 번아웃 등 다양한 요소로 good code를 작성하는 것은 미뤄지고 bad code들만 쌓이게 된다. 

> LeBlance's law: *Later equals never*

## What Is Clean Code?
애석하게도 bad/dirty code와 clean code를 구별해낼 수 있는 능력만으로는 **clean code를 짤 수는 없다**. Clean code를 구현하기 위해서는 소소한 부분에 있어서의 technique과 상당한 공을 들여야 한다. 또한 clean code란 무엇인지에 대한 명확한 정의도 존재하지 않기 때문에, 우리는 좋은 design 방법을 고민해야 한다. 다음은 여러 프로그래머들의 clean code에 대한 철학이다.

- elegant, efficient, straightforward logic 
- simple, direct, well-written == *readability*
- clear and minimal API, can be read

## Thoughts
사실 책의 제목만 봤을 때, 그리고 'clean code를 읽으면 좋다'라는 주변의 이야기들을 들었을 때 이 책이 나에게 더 많은 것들을 줄 수 있을 것이라 생각했다. 하지만 저자들은 Conclusion 파트에서 이 책이 당신을 좋은 프로그래머로 만들어 줄 수는 없을 것이라며 *미술 책을 읽는다고 미술가가 되지는 않는다*는 말을 근거로 가져왔다. 완전히 동의하는 말이지만, 만약 내가 좋은 코드와 나쁜 코드를 구별하는 *code-sense*를 타고나지 못했다 하더라도 최소한 좋은 코드의 조건을 알고 지금까지의 내 코딩 습관을 성찰할 수 있으면 한다.