---
title: "Java 8 - Method References"
tags: [java, method-references]
date: 2014-09-15T17:45:10+09:00
---

지난 포스팅에서 Lambda Expression을 통한 익명 메서드(anonymous method)를 생성하는 예를 간단히 살펴보았다. 이번에 살펴볼 'Method References'는 이미 존재하는 메서드를 함수형 인터페이스의 구현 내용으로 전달할 수 있는데, 조금 더 쉽고 가볍게 쓸 수 있는 syntax로 제공된다.

#### **Kind of Method References**

| 

 Reference to a static method

 | 

 ContainingClass::staticMethodName

 |
| 

 Reference to an instance method of a particular object

 | 

 containingObject::instanceMethodName

 |
| 

 Reference to an instance method of an arbitrary object of a particular type

 | 

 ContainingType::methodName

 |
| 

 Reference to a constructor

 | 

 ClassName::new

 |

- 표 1. Kind of Method References -

#### **Examples**

람다 표현식의 경우 함수형 인터페이스의 내용을 직접 구현하지만, '<u>메서드 참조(Method References)</u>'를 사용하면 기존에 구현된 메서드를 직접 참조할 수 있다. 다음은 '표 1'에서 나열한 메서드 참조의 4가지 종류에 대한 예제이다.

    class ComparisonProvider { public int compareByName(Person a, Person b) { return a.getName().compareTo(b.getName()); } public static int compareByAge(Person a, Person b) { return a.getBirthday().compareTo(b.getBirthday()); } } ComparisonProvider customComparator = new ComparisonProvider(); // lambda expression Arrays.sort(array, (a, b) -\> { return a.getName().compareTo(b.getName()); }); Arrays.sort(array, (a, b) -\> customComparator.compareByName(a, b)); Arrays.sort(array, (a, b) -\> ComparisonProvider.compareByAge(a, b)); // reference to an instance method of a particular object Arrays.sort(array, customComparator::compareByName); // eq customComparator.compareByName(a, b); // reference to a static method Arrays.sort(array, ComparisonProvider::compareByAge); // eq ComparisonProvider.compareByAge(a, b); Runnable runnable = System.out::println; // eq () -\> System.out.println()

- 예 1. Instance/Static Method 참조 -

람다 표현식은 인터페이스의 파라메터 리스트가 일치(e.g. (String a, String b) == (a, b))해야 하지만, 메서드 참조는 상세 타입(particular type)에 따라 어느정도 객체의 임의적 표현이 가능하다. 예를 들어 'String.compareTo(String another)' 메서드의 경우, 메서드 참조를 통해 'String::compareTo'로 표현될 수 있으며 내부적으로는 'a.compareTo(b)'와 같은 내용으로 재정의된다.

    String[] names = { "Barbara", "James", "Mary", "John", "Patricia", "Robert", "Michael", "Linda" }; Arrays.sort(names, String::compareTo); // eq Arrays.sort(names, (a, b) -\> a.compareTo(b));

- 예 2. 상세 형식을 따르는 임의 객체의 Instance Method 참조 -

Java에서는 생성자(Constructor)도 메서드처럼 다루므로 메서드 참조를 활용할 수 있다. 단, 인자 없는 기본 생성자(default constructor)만 가능하며 다른 예와 마찬가지로 컴파일 차원에서 타입 추론(type inference)은 그대로 적용된다. 아래 내용은 함수형 인터페이스 중 하나인 Supplier 인터페이스의 함수 전달에 대한 예이다. Supplier 인터페이스는 단순히 'T get()' 추상 메서드를 정의하고 있다.

    @FunctionalInterface public interface Supplier\<T\> { T get(); } // lambda expression Supplier\<Object\> supplier = () -\> { return new Object(); }; // reference to a constructor Supplier\<Object\> supplier = Object::new;

- 예 3. 생성자 참조 -

#### **Conclusion**

메서드 참조와 람다 표현식의 병행이 익숙해지면, 상당한 양의 부수 코드를 줄이고 간결한 코드를 유지할 수 있다.

