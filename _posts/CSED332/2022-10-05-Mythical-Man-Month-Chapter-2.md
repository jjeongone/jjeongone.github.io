---
layout: post
title: "[Mythical Man-Month] Chapter 2. The Mythical Man-Month"
date: 20221006
tag: [Book, SD]
published: true
---
*이 문서는 2022년도 가을학기 포스텍 박성우 교수님의 소프트웨어설계 수업을 들으며 Mythical Man-Month를 요약한 문서입니다. `Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다. 오타나 지적은 항상 환영합니다.* 😄

<hr>

많은 수의 software project는 일정 상의 문제로 실패한다. 왜 이런 문제가 발생하는 것인가? 그 원인을 분석해보면 다음과 같이 정리할 수 있다.

1. estimatimg technique이 충분히 발달하지 못하였다. 
2. estimating technique은 effort와 progress를 혼동하며, men과 month가 교환 가능하다고 가정한다.
3. 우리는 우리의 estimate에 확신이 없다.
4. 실제로 일정이 진행될 때 monitoring이 잘 이루어지지 않는다.
5. 일정이 불이행될 때 보통 전통적으로 manpower를 늘리는 방식으로 대처한다. 그리고 이는 문제를 가속화할 뿐이다.

## Optimism
대다수의 프로그래머들은 낙관론자이다.~~(ㅋㅋㅋㅋㅋㅋㅋ)~~ 이들은 보통 '이제 진짜 실행될거야'라고 하거나 '진짜 마지막 버그를 찾았어!!'라고 말한다. 그래서 우리가 scheduling을 함에 있는 하는 첫 번째 실수는 ***다 잘될거야***라는 생각이다.

*The Mind of the Maker*라는 책에서 Dorothy Sayers는 creative activity를 `Idea`, `Implementation`, `Interaction`의 세 단계로 나누었다. 그리고 인간이 수행하는 project의 경우 idea 수준의 모호함과 불확실성은 implementation 과정에서 보통 해소된다고 말한다. 

## Man-Month
보통 scheduling을 함에 있어 man과 month를 이용하여 cost를 계산하는데, 이는 상당히 위험하고 신뢰할 수 없는 신화이다. 이 신화에 따르면, 우리는 man과 month를 교환할 수 있어야 한다. Man과 month를 교환할 수 있는 유일한 조건은, task가 상호간의 dependency 없이 작게 쪼개질 수 있을 때 뿐이다. 그리고 이는 programming 상황과는 대게 맞지 않는다. Communication이 필요한 subtask들에 대해서는 업무에 드는 cost에 communication이 추가된다. 

Communication은 두 가지 측면으로 나눌 수 있는데, 하나는 train이고 다른 하나는 intercommunication이다. Train은 worker가 해당 업무에 익숙해지는 시간으로, 나누어질 수 없어 `# of worker`에 따라 linear한 값이다. Intercommunication은 인원의 수가 늘어날수록 상호간의 상호작용이 필요하기 때문에 `n(n-1)/2`만큼 증가한다. 이 모든 측면을 고려하였을 때 사람을 무작정 늘리는 일은 schedule을 늘리는 결과를 가져온다.

## Systems Test
System을 test하는데 드는 시간은 보통 mis-schedule되기 쉬운데, 이는 얼만큼 많은 error가 나올지 예측이 불가능할 뿐더러 우리가 code를 정확하게 잘 짤 것이라는 낙관에서 온다. 저자는 software task에 대해 다음과 같은 scheduling을 제안한다.

|portion|task|
|:---:|:---:|
|1/3|planning|
|1/6|coding|
|1/4|component test and early system test|
|1/4|system test, all components in hand|

Planning에 많은 portion을 할당한 이유는, 더 자세히 설계할수록 coding을 수행하는 과정에서 드는 effort를 최소화할 수 있기 때문이다. 그리고 전체의 절반 정도를 debugging에 할당하였는데, 이는 보통 delay가 일어나는 부분이자 실제 상업적으로 사용되는 program에서 상당히 세심히 살펴야 하는 부분이기 때문이다.

## Regenerative Schedule Disaster
만약 어떤 project가 계획에 비해 상당히 뒤쳐져 있다면 우리는 어떤 선택을 할 수 있을까? 보통은 manpower를 추가하는 선택을 할 것이다. Project를 시작하고 1개월 뒤에 delay를 발견한 상황을 가정해 보면, 우리가 할 수 있는 선택지는 다음과 같다.

1. 처음 계획되었던 대로 task를 완수할 것이라 가정한다. 초반의 계획만 잘못 설계되었을 것으로 생각하고, 이를 바로잡을 수 있을 만큼의 인원을 추가한다.
2. 처음 계획되었던 대로 task를 완수할 것이라 가정한다. 전체 계획이 잘못 설계되었을 것으로 생각하고, 이를 바로잡을 수 있을 만큼의 인원을 추가한다.
3. 전체에 대한 reschedule을 한다. 다시는 reschedule을 할 필요가 없도록 계획을 잘 한다.
4. Task를 다듬는다. 

처음 두 선택지는 정해진 일정동안 수행해야 하는 task에는 변화가 없기 때문에 여전히 disastorous하다. 빠르게 인원을 추가한다고 하더라도, 이들이 업무에 대해 배우고 적응하는 시간을 고려하면 촉박하다. 그리고 이미 분배되어 있던 task를 다시 쪼개는 일에도 비용과 노력이 요구되고, 쪼개진 subtask들 간의 dependency를 고려하면 필요한 man이나 month가 늘어날 수밖에 없다. 

**보통의 늦은 software project에서 manpower를 늘리는 일은 project를 더 늦게 만들 뿐이다.** Project를 수행하는 최대 인원은 independent한 subtask의 수와 같다. 즉, 적은 수의 man과 많은 양의 month가 이상적인 환경이라는 의미이다. 많은 수의 software project는 일정 상의 문제로 실패한다.
