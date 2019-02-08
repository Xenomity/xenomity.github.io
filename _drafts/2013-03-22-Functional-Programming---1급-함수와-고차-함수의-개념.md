---
title: "Functional Programming - 1급 함수와 고차 함수의 개념"
tags: [1급-함수, functional-programming, 고차-함수]
date: 2013-03-22T20:26:00+09:00
---

함수형 프로그래밍에서 함수를 '<u>1급 구성원<i>(first class citizen)</i></u>'으로 취급하는지에 따라 '함수형 언어'냐 '함수형 프로그래밍 스타일을 지원하는 언어(?)'냐로 구분하여 보는 것이 타당할 것 같다. 일반적으로 함수형 언어는 syntax 차원에서 함수를 1급으로 다룰 수 있다. 하지만 순수 함수형 언어가 아니더라도 부분적으로 함수 패러다임_(e.g. closure, currying, lambda, filter/map/reduce, etc...)_을 제공하는 언어들이 다수 존재하며 현재 지속적으로 다양한 언어들이 함수형 스타일을 받아들이고 있다. 일례로 Javascript나 Scala의 경우를 보면 -Object Oriented_(e.g. polymorphism, inheritance, encapsulation...)_-한 성격의 언어이지만 동시에 함수형 프로그래밍이 가능한 multi-paradigm 언어이자 함수를 1급 구성원으로 취급하고 있는 함수형 언어이기도 하다.

## First-Class Function (1급 함수)
함수를 1급으로 다룰 수 있는 프로그래밍 언어의 성질을 뜻한다. 여기서 다음 함수의 조건들을 만족하면 일반적으로 '1급 함수의 성질을 가진다'라고 말한다.
- 컴파일 시점이 아닌 실행 시점에 함수 생성이 가능해야 함.
- 함수 리터럴 지원. (function literal, 함수는 변수에 할당이 가능해야 함)
- 함수를 파라메터나 리턴 타입으로 전달이 가능해야 함.
```scala
def foo() { ... }

val fooFunc = foo // function literal
specObject.anotherFunc(fooFunc) // function delivery
```
- 예 1. Scala Functional Programming Style

Java의 경우 anonymous class를 통한 메서드 전달 방식으로 어느 정도 closure를 흉내낼 수 있지만, 컴파일된 모습만 보아도 이는 엄연하게 클래스 객체 전달로 보는 것이 맞으며 위에서 언급했듯이 함수_(java에서 stateless method를 함수로 본다면...)_ 자체의 변수 할당이나 생성 등이 불가능하므로 함수형 언어가 아니다. 차후 Java 8에서는 functional interface를 통해 함수형 스타일의 코딩을 지원하겠다고 하는데, 자세한 내용은 직접 봐야 알 듯... (hmm...)

## Higher Order Function (고차 함수)
고차 함수는 다른 함수를 생산/소비하는 함수를 말한다. 대표적인 고차 함수들은 아래와 같은 함수적 기능을 수행한다.
- **Map** : 데이터의 모든 요소를 전달받은 함수를 통해 가공하고 변환된 결과를 응답한다.
- **Filter** : 데이터의 모든 요소를 전달받은 함수를 통해 걸러내고 그 결과를 응답한다.
- **Reduce** : 가공된 데이터를 다양한 명령을 통해 결합하고 그 결과를 응답한다.

이 외에도 currying이나 memoization과 같은 기법들은 모두 전달받은 함수를 소비하고 응답할 함수를 생산하는 고차 함수로 분류할 수 있다.

