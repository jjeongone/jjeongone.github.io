---
layout: post
title: "[Clean Code] Chapter 4. Comments"
date: 20220913
tag: [Book, SD]
published: true
---
*이 문서는 2022년도 가을학기 포스텍 박성우 교수님의 소프트웨어설계 수업을 들으며 Clean Code를 요약한 문서입니다. `Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다. 오타나 지적은 항상 환영합니다.* 😄

<hr>

Comment의 존재는 필요악이라고 할 수 있다. 만약 우리가 적절하게 프로그래밍 언어를 사용하여 의미가 통하는 코드를 짠다면, comment는 필요없기 때문이다.

강조하건데, comment는 가능한 사용하지 않는 편이 최선이다.

Comment에 대한 평이 이렇게 박한 이유는, comment는 늘 코드를 잘 설명하지 못하기 때문이다. 처음 코드를 짜고 comment를 작성하였을 때에는 둘의 의미가 같을지도 모른다. 하지만, 시간이 지남에 따라 코드는 변화하는데 이 변화에 맞춰 comment까지 정확히 변화할지는 장담할 수 없다. ***오로지 우리가 신뢰할 수 있는 정보는 코드 뿐이다.***

## Comments Do Not Make Up for Bad Code
구현한 코드가 복잡하고 난해하다고 생각될 때 우리는 comment를 잘 작성하여 이를 무마하려고 한다. Comment를 잘 작성하기 위해 노력하는 시간을 code refactoring에 투자하는 것이 더 좋은 코드를 작성하는 길이다.

## Explain Yourself in Code
코드가 스스로의 기능을 잘 설명하지 못한다는 것은 착각이다. 다음 두 예시를 살펴보자. Raw한 코드와 주석을 읽는 것보다 길지만 잘 naming된 함수명을 읽는 편이 더 직관적이다.

```Java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

```Java
if (employee.isEligibleForFullBenefits())
```

## Good Comments
간혹 comment가 유용한 경우가 있다. 하지만 기억해야 하는 것은 여전히 comment를 쓰지 않는 것이 최선이라는 사실이다.

### Legal Comments
Copyright이나 Authorship과 같이 타당하고 필요한 내용은 comment에 담겨도 된다 ~~(담겨야 한다)~~.

### Informative Comment
정보를 담고 있는 comment는 다음과 같은 예시가 있다.
```Java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```
함수가 어떤 값을 return해주는지 comment를 통해 알려주는 방식은 유용하지만, 여전히 함수명에 기능을 녹여내는 편이 더 좋다. 예를 들어 위의 함수명을 `responderBeingTested`로 바꿀 수 있다.

```Java
// format matched kk:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = Pattern.compile(
    "\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```
위의 경우 어떤 format으로 값을 받는지 comment를 통해 알 수 있기 때문에 유용하다.

### Explanation of Intent
코드를 왜 그렇게 작성할 수밖에 없었는지 구성할 당시의 의도를 파악할 수 있는 comment는 다른 사람으로 하여금 코드를 이해하는 데에 도움을 준다.

### Clarification
Argument나 return값이 모호할 때에는 comment를 통해 확실히 하는 것이 도움이 된다. 처음부터 모호하지 않도록 코드를 짜면 좋지만, 외부 라이브러리를 import해서 쓰거나 수정할 수 없는 코드일 경우 comment를 사용하면 좋다.

### Warning of Consequences
Comment는 때로 코드 실행 결과에 대한 warning을 할 때 쓰인다. 예를 들어 다음과 같은 예시가 있다.

```Java
// Don't run unless you have some time to kill
public void _testWithReallyBigFile() {
    ...
}
```
요즘에는 `@Ignore` 테그를 통해 `@Ignore("Takes too long to run")`와 같이 적절한 설명을 할 수 있다. 

### TODO Comments
`//TODO` comment를 통해 *모종의 blocker*로 인해 진행되지 못한 부분에 대해 reminder를 하고  앞으로 해당 함수/코드가 어떤 기능을 하게 될 것인지를 명시할 수 있다. 

```Java
//TODO-MdM these are not needed
// We expect this to go away when we do the checkout model
protected VersionInfo makeVersion() throws Exception
{
    return null;
}
```

### Amplification
Comment는 다른 사람들에게는 중요해 보이지 않는 부분에 대한 강조를 하는 역할을 하기도 한다.
```Java
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting
// spaces that could cause the item to be recognized
// as another list.
...
```

## Bad Comments
대부분의 comment들이 이 category에 해당한다. Bad comment들은 대게 적절하지 못한 결정이나 구린 코드를 정당화하는 데 사용되지 때문에 피해야 한다.

- **Mumbling**: 만약 comment를 작성하기로 마음먹었다면, 심혈을 기울여 최선의 comment를 작성해라.
- **Redundant Comments**: Comment가 오로지 코드에 작성되어 있는 내용을 그대로 서술하는 것은 낭비이다. 오히려 길고 구어적인 comment가 코드보다 읽기 어려울 수 있다.
- **Misleading Comments**: 잘못 읽혀 오해의 소지가 있는 comment를 작성하지 않도록 주의하자.
- **Mandated Comments**: 모든 변수나 함수에 comment를 달아야 한다고 생각하지 말아라.
- **Journal Comments**: 모든 변화가 일어날 때 마다 일기 쓰듯이 일일히 주석을 다는 행위는 불필요하다.
- **Don't Use a Comment When You Can Use a Function or a Variable**: Comment에 작성된 내용을 function이나 variable을 통해 구현한 예시는 다음과 같다.
    ```Java
    // does the module from the global list <mod> depend on the subsystem we are part of?
    if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
    ```
    ```Java
    ArrayList moduelDependees = smodule.getDependSubSystems();
    String ourSubSystem = subSysMod.getSubSystem();
    if (moduleDependees.contains(ourSubSystem))
    ```
- **Position Markers**: Comment를 이용하여 특정한 위치를 알리는 marker를 만들고 싶을 수는 있지만, 보통은 거추장스럽고 제 기능을 하지 못한다.
- **Closing Brace Comments**: 중괄호가 끝나는 부분에서 어느 문(state)인지를 나타내는 주석을 달면 읽기 편할 수는 있지만, 차라리 코드를 간결하게 짜는 편이 좋다.
- **Attributions and Bylines**: 누가 작성한 코드인지 명시해두면 질문을 할 때 유용할 수는 있지만, 코드는 변화하고 사람도 유동적이기 때문에 비효율적이다. (git을 쓰면 누가 수정했는지 알 수 있으니 코드에 명시하는건 정말 별로인듯?)
- **Commented-Out Code**: 주석처리된 코드를 남겨두지 말아라. 누군가 주석 처리된 코드를 본다면 필요한 코드인지 그렇지 않은지 알 수 없기 때문에 차마 건드리지 못할 것이다.(옛날에는 필요했을 수도 있지만 지금은 전혀 아니다)
- **HTML Comments**: HTML로 작성된 comment는 programmer가 읽기에는 최악이다. 어떤 program이나 IDE를 거쳐 읽기 쉽게 반환되는 경우에는 몰라도, 사람이 읽을 것이 못된다.
- ***TMI(Too Much Information)***
- 

## Thoughts
주석이 없는 코드가 좋은 코드라는 말은 컴공 전공을 선택한 이후로 늘 듣고 있는 말이지만, 이렇게까지 주석을 싫어하는 글은 처음이었다. 주석이 귀가 있다면 귀를 막고 눈이 있다면 눈을 가려주고 싶을 정도로..😶

사실 1, 2학년때 들었던 전공 과목들에서는 주석을 필수적으로 작성하도록 하였다. 어느 정도로 자세히 적어야 감점이 되지 않을까 노심초사하며 잔뜩 주저리 적었던 기억이 나는데, 사실 감점되지 않기 위해서는 간결한 주석을 작성했어야 했을지도 모르겠다.

Naming의 중요성과 이렇게까지 긴 이름을 변수/함수명으로 정해도 되는지를 몰랐을 때에는 어떻게 주석이 없는 코드를 읽고 직관적으로 바로 이해할 수 있는지 의문이었는데, 적절한 naming이 갖추어져 있다면 충분히 readable한 코드를 짤 수 있을 것이라는 생각이 든다.

본문에도 나와있지만, **format**을 명시하는 주석은 중요한 요소라고 생각된다. Backend API를 짤 때 어떤 형식으로 값이 들어와야 하는지를 명시하지 않아 의도하지 않은 값이 들어와 문제가 발생한 경험이 있어 더욱 공감되었던 것 같다. 

~~사실 어쩌면 Good Comments까지만 읽고 그 외의 주석은 달면 안되는구나~ 하고 생각하고 넘어가도 괜찮았을 것 같기도 하다.~~

<hr>

## Reference
- Clean Code
