---
layout: post
title: "[Clean Code] Chapter 9. Unit Tests"
date: 20220928
tag: [Book, SD]
published: true
---
*이 문서는 2022년도 가을학기 포스텍 박성우 교수님의 소프트웨어설계 수업을 들으며 Clean Code를 요약한 문서입니다. `Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다. 오타나 지적은 항상 환영합니다.* 😄

<hr>

Chapter에 들어가기에 앞서 `Agile`과 `TDD`의 개념을 먼저 정리하고자 한다.

### Agile
> 1. 날렵한, 민첩한(=nimble) 2. (생각이) 재빠른, 기민한

사전적 의미에서 알 수 있듯, agile은 짧은 주기의 계획을 통해 하나의 큰 프로젝트를 완성해 나가는 과정을 말한다. Agile을 성공적으로 실행하기 위해서는 동료간의 적극적인 피드백과 소통이 중요하기 때문에, kanban보드를 사용하거나 daily scrum, weekly scrum 등을 도입하기도 한다.

### TDD(Test Driven Development)

Agile을 달성하기 위한 방법론 중 하나로, **테스트 케이스를 먼저 작성**하고 이를 달성할 수 있는 코드를 짜는 개발 방식을 말한다.

## The Tree Laws of TDD
TDD를 수행하기 위해 지켜야 하는 3가지 법칙은 다음과 같다.

1. Failing unit test를 작성하기 전까지 production code를 작성해서는 안된다.
2. 불필요할 정도로 많은 unit test를 작성할 필요는 없다.
3. Test를 통과하기 충분한 production code를 작성하면 된다. (== test에서 요구하는 것보다 많은 양을 개발하지 않아도 된다)

이를 지켰을 때 우리는 보다 효율적으로 test case를 작성하고 production code를 테스트할 수 있다.

## Keeping Tests Clean
Test code를 깔끔하게 작성하는 것의 중요성을 잘 알아차리지 못하는 경우가 많은데, **지저분한 test code는 test code를 안쓰니만 못하다**. Production code는 정적이지 않다. 시간이 지남에 따라 수정되고 발전한다. 이에 따른 test code의 수정이 필요한데, test code를 깔끔하지 않게 작성하였을 경우 이 작업에 많은 시간을 쏟아야 한다.

관리되지 않는 test code의 위험성은 다음과 같다. Production에 새로운 기능을 추가하기 위해서는 추가하는 code가 기존의 code와 충돌을 일으키지는 않는지, 의도한 대로 작동하는지 먼저 확인해야 한다. 너무 지저분해서 test code를 손볼 수 없는 상태에 이르면 이 확인 과정이 불가능해지고, production code는 발전하지 못한 채로 과거에 갇혀 썩어버리고 만다. 여기서 우리는 ***test code는 production code만큼 중요하다***는 사실을 알 수 있다.

### Tests Enable the -ilities
*Unit test*는 코드의 유연함과, 재사용성, 유지보수에 큰 도움이 된다. Test code가 커버할 수 있는 범위가 넓을수록 production의 자유도는 높아진다. 이런 이유에서 test가 `-ilities`를 가능하게 한다고 말한다. 

## Clean Tests
그렇다면 어떻게 해야 clean한 test code를 작성할 수 있을까? Clean test code에서 가장 중요한 점은 **Readability**이다. Test code의 readability는 production code에서보다 더 중요하다. 일반적인 코드를 작성할 때와 동일하게 명확하고 간단하며 의도한 바를 잘 나타내는 code를 우리는 readability가 있다고 말한다.

`BUILD-OPERATE-CHECK` pattern에 대한 이야기를 하는데 이 부분에 detail한 부분 찾아보기!

### A Dual Standard
Test code가 production code처럼 간단명료하고 명확해야 하지만, production만큼 효율적일 필요는 없다. Test code는 test environment에서 실행되기 때문에 고려할 부분이 약간은 다르다.

## One Assert per Test
한개의 test에서는 하나의 asserts만을 발생시켜야 한다는 관점이 있다. 복잡한 하나의 test를 여러개의 seperate tests로 나누면, 각각이 어떤 기능을 하는지 명확해진다는 장점이 있다. 반면, 오히려 코드의 반복이나 복잡성을 늘릴 수 있기 때문에 multi asserts을 사용하기도 한다. Multi asserts이라고 해서 마구잡이로 asserts을 쓰라는 뜻이 아니라 여러 asserts을 사용하되 최소한을 유지해야 한다는 의미이다.

`TEMPLATE METHOD` pattern에 대한 언급이 있음.

Asserts를 작성함에 있어 또다른 중요한 관점이 있다. 하나의 test function에서 여러 test를 진행하게 되면, 앞서 강조한 minimize asserts에 위배될 뿐만 아니라 readability를 감소시킨다.

정리하자면, 1. 최소한의 assert를 사용하고 2. 하나의 test function에서 하나의 concept에 대해서만 test를 진행해야 한다.

## F.I.R.S.T.
Clean test에 대한 rule은 다음과 같이 `F.I.R.S.T.`로 정리할 수 있다.

### **F**ast
Test code는 빨라야 한다. Production code를 작성하고 반복적으로 test code를 실행시켜 보며 오류를 찾고 디버깅을 수행해야 하기 때문에 느린 test code는 이에 부적합하다.

### **I**ndependent
각각의 test는 independent해야 한다. 하나의 test code가 다른 test code에 영향을 준다면 code를 디버깅하기 복잡해진다.

### **R**epetable
어떤 environment에서도 실행될 수 있어야 한다. Production environment와 QA environment 뿐만 아니라 test는 개인 laptop에서도 실행될 수 있어야 한다.

### **S**elf-Validating
Test는 성공 여부를 나타내는 `Boolean`값을 return해야 한다. 실행 결과를 일일히 비교하는 것은 테스트 결과를 재차 검증해야 하기 때문에 test code의 존재 의의에 맞지 않는다.

### **T**imely
적절한 타이밍에 test code를 작성해야 한다. Production code가 작성된 뒤에 test code를 쓰기에는 늦는다.

## Thoughts
생각해보면 지금까지 academic한 coding에서 test code를 작성해본 경험은 거의 전무한 것 같다. 남이 만들어 놓은 script나 예시 input을 사용해 본 적은 있지만, test code를 직접 작성하는 것은 이번 SD 수업을 들으면서가 처음이다. 말고는 서버를 코딩할 때 `NestJS`에서 제공하는 test code format 상에서 테스트를 해본 경험이 있다. 테스트의 중요성은 회사를 다니면서 여러 차례 경험을 했는데, 특히 게임 서버가 터지면 매출과 직결되어 있기 때문에 더욱 신중해야 했다. Test code를 production code 이전에 작성해야 한다는 개념은 처음 알게 되었는데, 직접 그 이유를 경험해 보고 싶다.

<hr>

## Reference
- Clean Code