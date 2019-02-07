---
title: "Java - strictfp"
date: 2010-05-02 08:26:28
tags: [java]
---

# Java - strictfp
자바 1.2부터 새롭게 추가된 `strictfp` 키워드는 부동소수점 연산을 이식성 있게 수행하도록 한다.  

JVM은 부동소수점 연산에 엄격하지 않아, 플랫폼이 JVM의 지수 크기(*e.g. float/double primitive type precision*)보다 더 큰 범위의 정밀함을 제공한다면 JVM은 플랫폼의 부동소수점 연산에 종속적으로 수행된다. 이것이 플랫폼에 따라 다른 결과값을 만들어 낼 수도 있다 보니, 정밀한 결과값에 대한 핸들링이 요구되는 경우에는 결과에 대한 일관성을 보장 할 수가 없다. 이런 상황에서 `strictfp` 키워드는 행위에 **엄격한 부동소수점(*Strict Flaoting Point*)** 정책을 적용하여 연산에 대한 일관성을 보장하도록 강제한다. 즉, 플랫폼과 상관없이 JVM의 부동소수점 연산을 사용하겠다는 것이다.

`strictfp`는 클래스, 인터페이스, 비-추상 메서드에 사용될 수 있다.

```
strictfp interface Fooable {}

public strictfp class Foo {}

public class Test {
	public strictfp void echo() {}
}
```


### References
* Wikipedia. [http://en.wikipedia.org/wiki/Strictfp](http://en.wikipedia.org/wiki/Strictfp)
