---
title: "Java 8 - Lambda Expression과 변경된 Interface의 모호함"
tags: [closure, functional-programming, java, lambda]
date: 2014-09-09T03:36:15+09:00
---

Java 8이 릴리즈된지 꽤 지났음에도 불구하고, 내 게으름으로 인해 이제야 Lambda 테스트를 해 보게 되었다. Lambda의 지원 여부를 놓고 오래 전부터 옥신각신(?)하던 과거와는 달리, java가 Oracle로 흡수되면서 보이는 첫 큰 변화라고 해도 과언이 아닐 듯 싶다. (만약 Sun이었다면 지금도 java의 객체지향적 특성과 현대적 프로그래밍 언어의 특성에 대한 혼잡성이라는 변화를 주저했을지도 모른다.)

무엇보다도 함수형 프로그래밍(Functional Programming)을 위한 지원이 어느 수준까지인지, Lambda Expression을 공식 지원하게 되면서 기존 API의 하위 호환성에 대한 부분을 어떻게 해결했을지가 가장 큰 궁금증이었다.

## Lambda Expression
함수형 언어에서 function은 'First-Level Class' 이므로 변수에 할당하고 파라메터로 전달될 수 있어야 하지만, Java는 함수형 언어가 아니므로 당연히 함수를 지원하지 않는다. 이전까지는 Anonymous Inner-Class(익명 내부클래스)를 통해 어느정도 Closure를 지원했지만, Java 8 에서는 -_단 하나의 메서드만 선언된 Interface_-만을 람다로 표현할 수 있도록 강제하여 함수형 프로그래밍을 가능하게 한다. 여기서 정의되는 인터페이스를 **함수형 인터페이스**라고 정의한다.
```java
    // Previous Java (Anonymous Inner-Class for Closure) new Thread(new Runnable() { @Override public void run() { System.out.println("Ohh~~"); } }.start(); // Java 8 Lambda Expression new Thread(() -\> System.out.println("Ohh~~")).start();
```

다음은 Java의 람다 표현식 및 람다 표현식을 사용하는 몇 가지 예이다.
```java
    // Syntax (int a, int b) -\> { return a + b; } (a, b) -\> { return a + b; } // 파라메터 타입 추론(parameter type inference). () -\> System.out.println("Ohh~~"); (String s) -\> { System.out.println(s); } () -\> 42 () -\> { return 3.1415; }; // e.g. Runnable r = () -\> System.out.println("Ohh~~"); Consumer\<Integer\> c = (int x) -\> { System.out.println(x); }; BiConsumer\<Integer, String\> b = (Integer x, String y) -\> System.out.println(x + " : " + y); Predicate\<String\> p = (String s) -\> { s == null; };
```

## Mark-up @Annotation for Functional Interface
Java 8은 위에서 언급했던 함수형 인터페이스를 명시하기 위해 `@FunctionalInterface`라는 추가 어노테이션을 제공한다. 실제로 단일 메서드를 선언하고 있는 어떠한 인터페이스도 람다 표현식으로 사용될 수 있지만, @FunctionalInterface를 통해 명시적으로 함수형 인터페이스임을 선언할 수 있다. 이는 인터페이스가 두 개 이상의 메서드를 선언하게 될 경우, 단순히 컴파일 오류를 발생시켜 주는 역할을 한다.
```java
    @FunctionalInterface public interface MyTask\<T\> { public T execute(T targetTask); }
```

참고로 'java.util.function' 패키지를 살펴보면 다양한 상황에서 람다 표현식을 활용할 수 있는 함수형 인터페이스들이 다수 정의되어 있다.

(See [http://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html](http://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html))

## Immutable Free-Variable
람다에서도 마찬가지로, 함수 외부에서 선언된 변수에 대한 값의 변경은 불가능하다. 이는 익명 클래스로 클로저를 흉내내던 이전 버전의 자바에서도 마찬가지인데, 명시적인 'final' 키워드로 포획된 변수(Captured Variable)에 대한 수정 불가함을 syntax 차원으로 제약했던 것과는 달리, Java 8의 람다 표현식에서는 'final' 키워드로 선언되지 않은 외부 변수도 포획이 가능하다. Java의 Closure에서 자유 변수는 불변이라는 법칙이 변하지 않은 가운데, 이 부분은 문법적으로 통일하지 않은 이유는 조금 이해가 가지 않는다... ^^;
```java
    // Previous Java (Anonymous Inner-Class) int counter = 0; new Thread(new Runnable() { @Override public void run() { System.out.print(counter); } // Compile Error!! }; // Java 8 Lambda Expression int counter = 0; new Thread(() -\> System.out.print(counter)); // Good. new Thread(() -\> System.out.print(counter++)); // Compile Error!!
```

### Defender Method
람다 표현식이 JDK 8에 포함되면서 추가적인 functional method들이 기존 인터페이스에 영향을 끼쳤을 것이다. 대표적으로 Collection Framework을 포함한 JDK의 수많은 클래스들은 복잡한 상속과 구현 체제에 얽매여져 있고, 하위 호환성을 안고 가기에는 어쩔 수 없는 선택으로 Java 8은 인터페이스에 구현 가능한 메서드를 'Default Method'라는 이름으로 추가하였다. 이는 기존 인터페이스를 구현하고 있는 클래스들에게 가상 확장 메서드(virtual expansion method)와 같은 역할을 한다.
```java
    @FunctionalInterface public interface Iterable\<T\> { Iterator\<T\> iterator(); default void forEach(Consumer\<? super T\> action) { Objects.requireNonNull(action); for (T t : this) { action.accept(t); } } }
```

변경된 인터페이스에 대한 자세한 내용은 'Oracle: Defining an Interface'([http://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html](http://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html))을 참고한다.

## Ambiguous Interface
인터페이스는 default method와 여기서 언급하지는 않았지만 정적 메서드(static method)를 포함하여 자체 구현을 가지게 되었다. 이로 인해 발생하는 투명성의 저하는, 구현체가 없이 규약으로서 의미를 지니는 기존 인터페이스에 비해 상당히 애매모호하다는 점과 다중 상속이라는 두 가지 문제를 야기시킨다. 이기종간의 동일한 인터페이스가 구현하는 내용은 오히려 제약적일 수 있으며, 이미 C++에서 충분히 겪어왔던 다중 상속의 문제점 중 하나로 하위 클래스가 부모 클래스를 알아야 한다는 원론적인 문제에 빠질 수 있다. 아래의 예는 다중 상속으로 인한 하위 클래스의 문제점을 표현한 코드이다.
```java
    public interface A { default void foo() { … } } public interface B { default void foo() { … } } public class Z implements A, B { @Override public void foo() { A.super.foo(); // or B.super.foo(); } }
```

## Conclusion
다시금 모던한 언어로 발돋움하고자 하는데 '뒤늦음'에 비해 '색다른' 해법이 보이지 않는다 생각하니 조금 안타까운 업데이트였다..


