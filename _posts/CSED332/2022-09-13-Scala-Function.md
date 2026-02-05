---
layout: post
title: "[Scala] Function"
date: 20220913
tag: [Scala, SD]
published: false
---

## Self-Reference

다음과 같은 class가 존재한다고 하자.

```Scala
class Rational(x: Int, y: Int) {
    private def numer = x
    private def denom = y
    
    def less(that:Rational): Boolean =
        this.numer * that.denom < that.numer * this.denom
}
```
이 때 `numer`은 private이지만, Rational을 type으로 가지는 다른 class에서도 접근이 가능하다. 이를 허용하지 않기 위해서는 다음과 같이 코드를 작성할 수 있다.

```Scala
class Rational(x: Int, y: Int) {
    private[this] def numer = x
    private[this] def denom = y

    def less(that: Rational): Boolean =
        this.numer * that.denom < that.numer * this.denom
}
```

## Essence of Object-Orientation
1. multiple representation, dynamic dispatch
   - object type에 따라서 compiler가 적절한 함수로 실행해주는 것
2. Encapsulation
3. Subtyping
4. Inheritance
5. Self-reference, open recursion

## Infix Notation = Method Invocation
Scala에서는 다음과 같은 방식으로 method를 부를 수 있다. 단, 주의해야 할 점은 두 방식을 섞어서 사용해서는 안된다는 점이다.
```
<object> <method> <argument> 
= <object>.<method>(<argument>)
```

method != function

## Use Assertions
- assert{}
  - state invariants on your code
  - 만약 `false`라면, 당신의 programming error일 것이다
- require{}
  - 만약 `false`라면, 해당 method를 사용한 사람의 실수일 것이다.
- ensuring{}
  - 만약 `false`라면, 당신의 programming error일 것이다
