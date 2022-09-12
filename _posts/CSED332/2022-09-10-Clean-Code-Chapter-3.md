---
layout: post
title: "[Clean Code] Chapter 3. Functions"
date: 20220910
tag: [Book, SD]
published: true
---
*`Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다.*

*오타나 지적은 항상 환영합니다.*

<hr>

초기 프로그래밍에서는 *routines*나 *subroutines*의 개념을 사용하여 system을 구성하였다. 이후 *programs*, *subprograms*, *functions*의 개념이 나타났고, 이들 중 *function*만이 현재 살아남아 사용되고 있다.

함수를 잘 사용하면 코드를 보다 이해하기 쉽게 해주고, 의도를 직관적으로 이해할 수 있게 해준다. 그렇다면, 함수를 **잘**사용한다는 것은 어떤 것일까?

## Small!
명확히 증명할 수는 없지만, function은 항상 **작은 것**이 좋다. 함수에 대한 첫 번째 규칙은 작아야 하는 것이고, 두 번째 규칙은 *그것보다 더 작아야* 한다는 것이다! 

작은 함수는 얼마나 작아야 할까? 다음 두 함수를 살펴보자.

```Java
public static String renderPageWithSetupandTeardowns(PageData pageData, boolean isSuite) throws Exception {
    boolean isTestPage = pageData.hasAttribute("Test");
    if (isTestPage) {
        WikiPage testPage = pageData.getWikiPage();
        StringBuffer newPageContent = new StringBuffer();
        includeSetupPages(testPage, newPageContent, isSuite);
        newPageContent.append(pageData.getContent());
        includeTeardownPages(testPage, newPageContent, isSuite);
        pageData.setContent(newPageContent.toString());
    }
    return pageDate.getHtml();
}
```

```Java
public static String renderPageWithSetupandTeardowns(PageData pageData, boolean isSuite) throws Exception {
    if (isTestPage(pageData))
        includeSetupAndTeardownPages(pageData, isSuite);
    return pageData.getHtml();
}
```

둘 중 하나를 선택하라고 한다면, 당연히 후자일 것이다. 놀라운 점은 전자 또한 refactoring을 거친 version이라는 것이다. 두 번째 code-block은 첫 번째 code-block의 refactoring version으로 원본 코드에 대한 *re-refactoring version*이라 할 수 있다. **함수를 줄이는 데에 한계란 없다**. ~~(함수가 존재하는 선에서 말이다)~~

### Blocks and Indenting
`If-else`문이나 `while`문은 한 줄 정도가 적당하다. 조건을 검사할 때 보통 어떤 연산을 수행하게 되는데, 이를 적절한 함수를 통해 실행하면 조건문에 대한 *documentation* 효과를 볼 수 있다.

## Do One Thing
함수에 대한 강력한 조언은 다음과 같다.
> Functions should **do one thing**. They should **do it well**. They should **do it only**.

그렇다면, 여기서 강조하는 **one thing**이란 무엇일까. 단 하나의 기능(예를 들면 비교)만을 수행해야 하는 것일까? 그렇지 않다. 함수를 *여러 step*으로 구성될 수 있으며, 이 step들이 모여 수행하는 기능을 한 문장으로 요약할 수 있다면 하나의 기능이라고 볼 수 있다.

## One Level of Abstraction per Function
### Reading Code from Top to Bottom: *The Stepdown Rule*
사람들은 code를 위에서 아래로 읽어 내려가는 흐름대로 이해하고 싶어할 것이다. 이를 *The Stepdown Rule*이라 부르고, 저자들은 *TO* paragraph을 읽는 순서와 동일한 흐름이라고 서술한다. 한국어로는 `~하기 위해 -를 한다`정도로 이해를 하면 쉽다.

이런 흐름을 유지하면서 코드를 작성하기는 쉽지 않지만, 함수가 **do one thing**을 유지하는 데에는 효율적이다.

## Swith Statement
~~*(뭔가 읽어도 이해가 잘 안되는 파트인데 Java를 잘 몰라서인지 내가 이해를 못한것인지)*~~

**Do one thing**을 하지 않는 대표적인 예시로 `switch`문이 있다. 보통 ~~(어쩌면 항상)~~ `switch`문은 *do N things*를 수행한다.

다음과 같은 `switch`문을 사용하는 프로그램이 있다고 할 때 이의 한계점은 아래와 같다.

```Java
public Money calculatePay(Employee e)
throws InvalidEmployeeType {
    switch (e.type) {
        case COMMISSIONED:
            return calculateCommissionedPay(e);
        case HOURLY:
            return calculateHourlyPay(e);
        case SALARIED:
            return calculateSalariedPay(e);
        default:
            throw new InvalidEmployeeType(e.type);
    }
}
```

1. 새로운 employee type이 추가되면 `switch`문은 더 길어질 것이다
2. **Do *not* one thing**임이 확실하다
3. Single Responsibility Principle (SRP)를 위반한다
4. Open Closed Principle (OCP)를 위반한다

이러한 문제를 해결하기 위한 방법은 다음과 같다. `switch`문을 *abstract factory*에 넣어 사용자로 하여금 보이지 않게 한다. 이 `switch`문은 employee에 따른 적절한 수행을 matching시키는 데에 사용된다.

```Java
public abstract class Employee {
    public abstract boolean isPayday();
    public abstract Money calculatePay();
    public abstract void deliverPay(Money pay);
}

public interface EmployeeFactory {
    public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

public class EmployeeFactoryImpl implements EmployeeFactory {
    public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
        switch (r.type) {
            case COMMISSIONED:
                return new CommissionedEmployee(r);
            case HOURLY:
                return new HourlyEmployee(r);
            case SALARIED:
                return new SalariedEmployee(r);
            default:
                throw new InvalidEmployeeType(r.type);
        }
    }
}
```

## Use Descriptive Names
함수의 이름을 정하는 것은 중요하다. 다양한 후보 중에서 함수의 기능을 가장 잘 설명할 수 있는 이름을 선택해야 한다. *clean code*의 절반은 **do one thing**을 하는 작은 함수를 설명할 수 있는 이름을 짓는 것이다. 좋은 이름을 찾기 위한 조언은 다음과 같다.

- **긴 이름에 대한 두려움**을 버려라: 설명을 잘 할 수 있는 긴 이름은 수수께끼같은 짧은 이름보다 훌륭하다. 긴 descriptive name은 긴 descriptive comment보다 좋다.
- 이름을 정하는 데 **긴 시간**을 투자하는 것을 당연시해라: 다양한 이름을 코드에 적용시켜 보고, 제일 적합한 이름을 찾아가는 것을 추천한다. 제공되는 IDE의 기능을 활용하면 편하게 refactoring할 수 있다.
- **어휘를 통일**시켜라: Chapter 2의 *Pick One Word per Concept*에서도 강조한 바 있듯, 같은 *phrase*, *nouns*, *verbs*를 이용하여 이름을 지어야 나중에 코드를 읽고 이해하기 쉽다.

## Function Arguments
적절한 함수 인자의 수는 몇 개일까? 이상적인 함수 인자 수는 0개이다. 1개나 2개, 3개까지는 특수한 경우에 용인되지만, 그 이상은 지양해야 한다. 함수 인자 수가 적어야 하는 이유는 함수 인자의 수가 늘어남에 따라 computing power가 많이 들고, test를 하기 어려워지기 때문이다. 최대한 직관적인 함수를 구성하기 위해 **가능한 적은 수의 인자**를 받도록 설계해야 한다.

### Flag Arguments
함수의 인자로 `boolean`값을 넘겨주는 것은 최악이다. 왜냐하면 `boolean`값을 인자로 받는다는 의미는, T/F 여부에 따라 다른 행위를 한다는 뜻이기 때문에 **do one thing**을 위반한다.

### Dyadic Functions
인자가 2개인 함수는 인자가 1개인 함수에 비해 함수의 기능을 직관적으로 이해하기 어렵다. 예외적인 상황이 있는데, 시간을 나타내는 함수이다. 시간과 분을 각각 입력받는 함수는 인자가 두개일 수밖에 없고, 동시에 직관적으로 확실하다. 하지만 실제로 이 *시간*이라는 개념은 하나의 값이고, 이를 순차적으로 서술한 것이기 때문에 single argument와 다를 바 없다. 만약 *monad*와 *dyadic* 중 선택하여 설계할 수 있다면 computing cost와 advantage에서 오는 cost를 잘 따져봐야 한다.

### Argument Objects
만약 어떤 함수가 3개 이상의 인자를 필요로할 때에는 그 인자들의 일부를 object로 받아야 하는 경우가 빈번하다. 예를 들어 다음과 같은 함수 정의가 있다고 하자. 직관적으로 두 번째 함수가 기능을 설명하는 데에 더 명확하다.

```Java
Circle makeCircle(double x, double y, double radious);
Circle makeCircle(Point center, double radious);
```

### Verbs and Keywords
좋은 함수명과 인자명 ~~(함수 인자의 이름)~~ 은 **함수의 기능을 설명**하는 데 도움을 준다. 예를 들어 `writeField(name)`이라는 함수가 존재한다면, 이는 직관적으로 name을 write하는 함수인데 이 name이 field와 관련되어 있다는 것을 알 수 있다.

## Have No Side Effects
함수를 실행하였을 때 side effects가 없도록 하는 것은 **do one thing**을 지키는 데에 필수적이다. 의도치 않은 변화가 발생하면, 사용자가 의도한 대로 code가 작동하지 않을 수 있다. 만약 한 함수에서 두 가지 일을 하고 싶으면, 그 둘을 모두 포함하는 naming을 하여 사실상 하나의 기능을 하는 함수를 구성해야 한다.

## Command Query Seperation
함수는 보통 *무언가를 수행*하거나 *무언가에 대답*하는 기능을 수행하는데, 이 둘을 동시에 하지 않아야 한다. 

## Prefer Exceptions to Returning Error Codes
Command 함수(무언가를 수행하는 함수)에서 error code를 반환하는 것은 *Command Query Seperation*을 위반하는 좋은 예시이다. `if-else`문을 통해 error code를 처리하는 것은 각각의 case마다 검사하는 logic을 구성해야 해서 code가 복잡해질 수 있는 반면, exception(`try/catch`문)을 사용하면 보다 간결하게 code를 구성할 수 있다.

```Java
try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
    logger.log(e.getMessage());
}
```

### Extract Try/Catch Block
`try/catch`문을 깔끔하게 작성하는 방법은, 기능을 분리하는 것이다. 위의 *Prefer Exceptions to Returning Error Codes*에서 구현된 code는 `try/catch`문 안에 함수의 기능이 포함되어 있어 자칫 code의 구조를 이해하기 어려워질 수 있다. 이는 다음과 같은 방식으로 code를 작성하면 해결된다.

```Java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    }
    catch (Exception e) {
        logError(e);
    }

    private void deletePageAndAllReferences(Page page) throws Exception {
        deletePage(page);
        registry.deleteReference(page.name);
        configKeys.deleteKey(page.name.makeKey());
    }

    private void logError(Exception e) {
        logger.log(e.getMessage());
    }
}
```

### Error Handling Is One Thing
**Error handling**은 **one thing**이다. 즉, error handling을 수행하는 `try/catch`, `try/finally`문 뒤에는 다른 기능을 하는 code가 와서는 안된다.

## Structured Programming
> Dijkstra's Rules of Structured Programming: every function, and every block within a function, should have one entry and one exit.

위의 *Dijkstra's Rule of Structured Programming*이 의미하는 바는, 함수에서는 단 하나의 `return`이 존재해야 하고, loop에서 `break`이나 `continuous`가 등장하지 않아야 하며, `goto`문은 사용해선 안된다는 뜻이다.

*Structured programming*을 하면 좋지만 우리의 목표는 어디까지나 작고 명확한 함수를 만드는 것이기 때문에, 이를 달성하기 위해 적절한 곳에서 `return`을 여러번 하거나 `break` 혹은 `continuous`를 사용하는 것은 적절하다. 하지만 여전히 `goto`문을 사용하는 것은 작은 함수를 만드는 데에 걸림돌이 되기 때문에 피해야 한다.

## How Do You Write Functions Like This?
**소프트웨어를 작성한다는 것은 여타 다른 글을 작성하는 것과 동일하다**. 우리는 글을 쓸 때 처음에는 두서없이 생각을 뱉어낸 후, 이를 다음고 정리하여 글을 완성한다. 함수를 작성할 때에도 마찬가지이다. 처음에는 길고, 인자의 수도 많으며, 추상적인 이름들을 가지지만, 이를 바탕으로 *여러 함수로 쪼개고*, *이름을 적절하게 수정하고*, *반복되는 부분을 삭제*하는 등 함수를 다듬는 과정을 거친다.

## Thoughts
지난 *Chapter 2*에 비해 더 많은 자아성찰의 시간을 가지게 한 장이었다. 함수를 쓰는 목적이 기능을 분리하고 재사용성을 높이는 데에 있다는 수박 겉핥기 식의 개념만 알고 있었지, 함수의 개념이 이렇게까지 고도화되어 있을 것이라 생각하지 못하였다. 지금까지 내가 만든 함수들의 추상적인 이름과 수많은 인자들이 조금은 부끄러울 지도 모르겠다. 적절한 `try/catch`문의 사용도 실제 코딩을 통해 배우고 싶고, 함수의 인자를 최적화해가는 과정도 경험하고 싶다. 

사실 refactoring에 대한 은연중의 망설임이 있었다. 지금까지는 단발적으로 사용하는 코드들을 짜오다 보니 형태가 마음에 들지 않아도 기능만 잘 하면 된다는 생각이 강했는데, 이렇게 어물쩡 넘겨버린 좋은 코드를 연습할 수 있는 기회를 앞으로는 놓치지 않으려고 한다.

적절한 이름을 위한 고민은 끝이 없을 것 같다.
