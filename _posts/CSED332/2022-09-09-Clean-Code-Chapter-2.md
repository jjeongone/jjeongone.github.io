---
layout: post
title: "[Clean Code] Chapter 2. Meaningful Names"
date: 20220909
tag: [Book, SD]
published: true
---
*`Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다.*

*오타나 지적은 항상 환영합니다.*

## Use Intention-Revealing Names
직관적인 naming은 상당히 중요하다. Variable이나 function, class 등의 이름은 **왜 그것들이 존재**하고 **어떻게 사용**될 수 있는지를 드러낼 수 있어야 한다.

예를 들어 개수를 저장하는 변수에 대해 `a`와 같은 의미 없는 문자가 아니라 `num`, `number`, `count`와 같이 변수명만 봐도 어떤 역할을 하는지 알 수 있도록 해야 한다.

또한 **함수를 적절히 사용**하여 부등식이나 등호를 이용한 계산이 아닌, 어떤 조건을 검색하는 것인지를 전달할 수 있다. 다음 두 코드를 살펴보자.

```Java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell: gameBoard)
        if (cell[STATUS_VALUE] == FLAGED)
            flaggedCells.add(cell);
    return flaggedCells;
}
```

```Java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell: gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
```

## Avoid Disinformation
Naming을 함에 있어 disinformation이 발생하는 경우는 다음과 같다.

- 실제 전문 용어로 사용하는 용어를 헷갈리게 사용하는 경우
  - ex. `List`구조를 사용하지 않는 group의 변수명에 'List'를 사용
- 소문자 `L`과 대문자 `o`를 변수명으로 사용하는 경우
  - ~~장난인가..?~~
- ... *무언가 세상에 다채로운 방식으로 존재*

## Making Meaningful Distinctions
Naming을 할 때 **의미 있는 단어**로 구성을 해야 한다. `a1`, `a2`, `a3`, ..., `aN`과 같은 number-series naming을 할 경우 이름을 짓기는 쉽지만 어떤 정보를 담고 있는지에 대한 정보는 전혀 들어있지 않다. 또한 불필요한 단어 사용을 주의해야 한다. 변수의 이름에 *variable*을 포함시키거나 table의 이름에 *table*을 넣는 등의 행위는 오히려 변수의 역살을 헷갈리게 만든다. **따라서 특정한 naming convention이 요구된다.**

## Use Pronounceable Names
**발음할 수 있는 단어**를 이용하여 naming을 하는 것은 소통에 있어 중요하다. 프로그래밍은 혼자서 모든 일을 해낼 수 없기 때문에 동료와 토론하는 상황이 불가피하다. 발음할 수 없는 이름을 가진 변수, 함수, 객체는 이런 상황에서 곤란함을 가져다 준다.

## Use Searchable Names
**검색 가능한** naming을 하는 것 또한 고려해야 하는 부분이다. 프로젝트가 커질수록 어느 파일에 어느 함수가 있는지 완벽히 파악하기 어려워지고, IDE에 포함된 검색 기능을 사용하게 된다. 만약 변수명이 `e`와 같이 영어에서 흔하게 사용되는 문자일 경우, 필요한 내용까지 도달하는 데에 오랜 시간과 노력이 필요하다.

## Avoid Encodings
축약어나 특정한 방식으로 encoding한 변수명을 사용하는 것은 개발자로 하여금, 특히 신입 개발자에게, 부담을 줄 수 있다. 동시에 발음이 어렵거나 오타를 내기 쉽다는 단점이 있다.

## Hungarian Notation
초기 Windows C API를 구성함에 있어 HN(Hungarian Notation)은 상당히 중요한 역할을 하였다. HN은 compiler가 type check를 수행하지 않기 때문에, 이름을 통해 type을 파악할 수 있는 *type에 따른 명명법이나 prefix*를 의미한다. 시간이 흐름에 따라 다양한 언어나 IDE에서 compile을 하기 전에 type check를 수행할 수 있게 되면서 HN은 오히려 코드를 읽기 어렵게 하고, type이 바뀔 때마다 변수명을 바꿔야 하는 불편함이 있기 때문에 이제는 사용하지 않는 방식이다.

요즘의 트랜드는 **더 작고 간결한 class, function**을 통해 각 변수들이 어떤 역할을 하는지 한눈에 알기 쉽도록 하는 것이다.

## Avoid Mental Mapping
Mental mapping이라 하면, 코드를 읽고 이해하는 데에 필요한 정신력을 말한다. 이는 `i`, `j`, `k`와 같은 (앞서 말했듯 소문자 `L`은 변수명으로는 부적합하다) single-letter variable name을 사용할 때 빈번하게 발생한다. *Smart programmer*과 *professional programmer*의 차이는 간결하고 명료한 코드의 중요성을 아는지에 따라 달려있다. 당연히 professional programmer 쪽이 **다른 사람들이 이해하기 쉬운 코드**의 중요성을 알고 있다.

## Class Names
Class의 이름을 정할 때에는 `Manager`, `Processor`, `Data`, `Info`와 같은 다른 기본 function에서 자주 쓰는 단어나 *verb*를 피하고, `Customer`, `WikiPage`, `Account`와 같은 ***noun***이나 ***noun phrase***를 사용해야 한다.

## Method Names
Method의 이름은 `postPayment`, `deletePage`, `save`와 같은 ***verb***나 ***verb phrase***를 사용해야 한다. 또한 `get`, `set`, `is`와 같은 prefix를 사용함으로써 method의 기능을 파악하기 쉽다.

**Static factory method**를 사용하면, `new`를 통해 객체를 생성하는 방식에 비해 더 직관적으로 코드를 이해할 수 있다. 다음 코드를 통해 살펴보자.

```Java
Complex fulcrumPoin = Complex.FromRealNumber(23.0);
```

```Java
Complex fulcrumPoint = new Complex(23.0);
```

## Don't Be Cute
Naming을 하다 보면 *재치있고 창의적인* 이름을 짓고 싶을 때가 있을 수도 있다. 특히 *구어적 표현*이나 *은어*, *culture-dependent joke*를 사용하면 해당 문화를 공유하는 사람들끼리는 즐거움을 줄 것이다. 이 책에 나온 예시는 특히 영어권이 아닌 우리 ~~(나만 그럴 수도..)~~ 들에게 좋은 예이다. `whack()`이나 `eatMyShorts()`라는 이름을 가진 함수가 어떤 기능을 하는지 직관적으로 알 수 있는가? 구글 번역기에 따르면 `whack`은 *구타*이고, `eat my shorts`는 심지어 1980년대에 나온 영화에서 생겨난 meme으로 상대방의 말에 *경멸*, *반박*, *거부*하는 맥락에서 사용되는 말이다. 자, 모두가 알아들을 수 있는 말을 사용하도록 하자.

## Pick One Word per Concept
Naming을 함에 있어 어휘를 통일시켜야 한다. 만약 서로 다른 class에서 `get`, `fetch`, `retrieve`라는 이름을 가지는 함수가 있다고 하자. 이들이 같은 기능을 하는지, 어떤 class에서 어떤 이름을 사용했는지 모두 외우기는 불가능하다. 최근 IDE인 *Eclipse*나 *IntelliJ*는 context에 맞는 method들의 list를 제공해 주지만, 이것에 의존해서는 안된다. **한결같은 어휘**는 당신의 코드를 읽어야 하는 누군가에게 도움이 된다.

## Don't Pun
같은 단어를 두 가지 유사한 기능에서 사용하지 않도록 하자. 이는 앞서 설명한 *"one word for concept"* 를 수행함에 있어 발생하기 쉬운 실수이다. 예를 들어 지금까지 `add`라는 함수를 *새 변수를 추가하거나 두 값을 합치는 기능*을 수행하도록 구성하였다고 하자. 만약 *새 변수를 추가*하는 기능만을 가지는 함수를 새로 만든다면, `add`라는 이름을 사용해도 되는가? **안 된다.** 유사해 보이지만 둘의 기능은 명확히 다르고, 코드를 사용하는 사람의 입장에서 헷갈림을 줄 수 있다. `insert`나 `append`라는 다른 어휘를 사용하면 이를 해결할 수 있다. *읽기 쉬운 코드를 작성하는 것은 중요하다.*

## Use Solution Domain Namaes
사실, 당신이 쓰는 코드를 읽는 사람은 높은 확률로 ~~(아마 확실히)~~ 프로그래머일 것이다. 이는 당신이 선택하는 어휘가 꼭 이해하기 쉬운 말일 필요는 없다는 뜻이다. 아마도 이들은 *computer science terms*, *algorithm terms*, *pattern names*, *math terms*와 같은 전문 용어를 어렵지 않게 이해할 수 있을 것이다.

## Use Problem Domain Names
만약 당신 주변에 프로그래머들이 없다면, problem domain names를 사용하는 것을 권장한다.

~~(사실 이 부분은 잘 이해가 되지 않음. 그냥 문제 상황에서 쓰이는 용어를 사용하면 좀 더 직관적일 수 있다는 말일까?)~~

## Add Meaningful Context
만약 `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, `zipcode`와 같은 변수들이 있다고 생각해 보자. 그렇다면 이들이 주소를 나타내는 변수라는 것을 쉽게 파악할 수 있을 것이다. 그렇다면 만약 `state`만 있다면, 과연 이 변수가 어떤 정보를 저장하는지 맥락을 쉽게 파악할 수 있을까?

이를 쉽게 해결하는 방법은 *의미 있는 prefix*를 추가하는 것이다. `addr`이라는 prefix를 추가하면, 쉽게 주소를 나타냄을 알 수 있다. 혹은 `Address`라는 이름을 가지는 class의 하위에 변수를 저장하면, 사람은 물론이고 compiler 또한 의미를 알 수 있게 된다.

하지만 아무리 맥락 정보를 충분히 제공한다고 하더라도 함수가 너무 길고 복잡하면 어떤 기능을 하는지 쉽게 파악하기 힘들다. 물론 함수명을 통해 어떤 기능을 수행하는지 추정할 수는 있지만, 내부 코드를 읽고 디버깅 해야 하는 상황에는 부적합하다. 이를 해결하기 위해서는 *sub-function*을 분리하여 조건문이나 특정 연산이 어떤 기능을 하는지를 명확히 할 수 있다.

## Don't Add Gratuitous Context
하지만! 동시에 너무 과한 맥락 정보를 naming에 사용하는 행위는 자제해야 한다. 이름은 직관적이고 의미를 잘 전달해야 하지만, 동시에 **간결**해야 하기 때문이다.

## Thoughts
당연하다고 생각했던 내용, 경험적으로 불편하게 느꼈던 방식(소문자 `L`을 변수명으로 사용하는 것)을 포함하여 코딩을 함에 있어 *가장 기초*라고 할 수 있는 이름 짓기에 대해 고민을 해볼 수 있는 장이었다. 또한 모든 꼭지들에서 **남들이 읽기 쉽고, 이해하기 쉬운 코드**에 대한 강조를 강하게 하고 있었다. 사실 대학교에서 과제를 할 때에는 보통 개인 과제이고, 팀프로젝트를 하더라도 규모가 그렇게 크지는 않기 때문에 코드를 작성함에 있어 readability를 고려할 필요성이 거의 없었다. 현재로써는 과거에 작성한 코드를 봐야 할 일이 거의 없지만, 앞으로라도 미래의 나를 위해 좀 더 신경써서 코딩을 해야 겠다는 생각이 들었다.