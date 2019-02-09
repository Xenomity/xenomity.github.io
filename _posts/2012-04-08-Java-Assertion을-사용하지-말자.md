---
title: "Java Assertion을 사용하지 말자"
date: 2012-04-08 08:26:28
tags: [java, assert]
---

## Problem
Java 1.4에서 추가된 `assert` 식은 다양한 사전 조건이나 제약 사항에 대한 검사를 편하게 할 수 있도록 예약어로 제공된다. 하지만 `assert` 식은 개인적으로 사용하지 않는 편이다. 그 이유는 `assert` 식의 대부분은 예외로 처리할 수 있으며, 여러모로 구시대적인 방식이라 할 수 있는 몇 가지 이유가 있다고 보기 때문이다.

다음은 몇 가지 이유를 추린 내용이다.

### 1. 검사 실패 시, `java.lang.AssertionError`를 발생시킨다.
이는 실행 시점(*runtime*)에 발생하는 오류이지만, `RuntimeException`과는 다르다. 해당 오류를 의도적으로 `try-catch` 문을 통해 후처리를 할 수는 있지만, 명시적으로 예외를 통해 처리하는 것보다 나은 점이 없다.

#### `assert` 문을 통한 사전 조건 검사의 예.

```java
public void foo(int number) {
  assert number >= 0;
}

----------
Exception in thread "main" java.lang.AssertionError
```

#### 예외를 통한 사전 조건 검사의 예.

```java
public void foo(int number) {
  if (number < 0) {
    throw new IllegalArgumentException("invalid number size.");
  }
}

----------
Exception in thread "main" java.lang.IllegalArgumentException: invalid number size.
```

### 2. 검사 실패 시, 해당 쓰레드는 종료된다.
앞서 말한 것처럼, `AssertionError`를 `try-catch` 하지 않는다면, 해당 쓰레드의 코드는 더 이상 실행되지 않고 종료된다. 만약 검사 조건이 실패하더라도 프로그램은 계속 실행되어야 한다면, 일일히 `assert` 문 마다 `try-catch` 문을 사용해야 할 수도 있다. 차라리 상황에 맞는 예외 처리나 throwing 기법이 더 명확하다.

### 3. Assertion 식은 default가 아니다.
JVM은 기본적으로 assertion 옵션이 비-활성화 상태이다. 각 JVM마다 assertion 사용 여부를 다르게 설정할 수 있다는 것인데, 이는 개인적으로 장점으로 보기 어려운 부분이다. 보통 assertion을 통한 조건 검사는 개발이나 테스트 환경에서 더 이상 실패가 발생하지 않는다고 하여 운영 환경에서 비활성화 하기에는 매우 위험하기 때문이다. 충분히 테스트 완료 된 assertion 이라도, 운영 환경에서 항상 옳은 조건 또는 값이 넘어온다는 보장은 없다.

결론적으로 가독성이 좋고 좀 더 견고한 검사 코드를 만들기 위해서는 Java Assertion이 아닌, 예외를 통한 검사 코드나 예외 수준(*exception-level*)의 `Assertion` 라이브러리 사용을 권장한다. 이미 다양한 프레임워크(*e.g. Spring, JUnit...*)에서는 검사 코드를 위한 예외 수준의 assertion api를 제공하고 있다.
