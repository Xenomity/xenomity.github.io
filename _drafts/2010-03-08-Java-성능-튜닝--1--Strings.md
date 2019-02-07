---
title: "Java 성능 튜닝 -1- Strings"
tags: [java]
Date: 2010-03-08T17:20:00+09:00
---

#### **1. String Class**

---
| |  Synchronized |
|  String |  O |
|  StringBuffer |  O |
|  StringBuilder |  X |
---

3가지 클래스 모두 내부적으로 private char[]를 가지고 있으며, 이 문자 배열을 통해 문자열을 보관한다. 단, String Class는 Immutable(불변)객체이므로 문자열 가공이 원천적으로 불가능하며, Java에서는 사용편의성을 위해 연산자 오버라이딩을 통한 문법으로서의 방법을 제시한다.   
  
예)
```
String str = "가" + "나" + "다";
```
  
그러나 String은 불변객체이므로, 실제 컴파일러는 위 코드를 StringBuffer로 치환하여 컴파일한다.  

```
String str = (new StringBuffer()).append("가").append("나").append("다").toString();
```
  
여러 문자열의 조합은 불필요한 객체 생성을 초래하므로 StringBuffer를 사용하는 편이 훨씬 이득이며, 다중 쓰레드 환경에서의 문자열 공유가 아니라면 동기화되지 않은 StringBuilder를 이용하는 것도 고려할 수 있다. StringBuffer와 StringBuilder는 동기화/비동기화의 차이뿐이다.  
  
테스트 결과 1)  

    // 1877ms String str = "abc"; for(int i = 0; i \< 10000; i++) { str += "z"; } // 6ms StringBuffer str = new StringBuffer(); for (int i = 0; i \< 10000; i++) { str.append("z"); }

  
테스트 결과 2)  
```
// 문자열 연산 : 1,232ms
for (int i = 0; i \< 10000; i++) {
  System.out.println("abc" + str);
}

// 문자열 비연산 : 992ms
for (int i = 0; i \< 10000; i++) {
  System.out.println("abc");
  System.out.println(str);
}
  

#### **2. StringBuffer/StringBuilder Capacity**
StringBuffer의 기본 생성자를 통해 객체를 생성한 경우, 16개의 문자 배열을 가진 객체가 생성된다. Default Capacity는 16이며, 이 크기보다 큰 문자열 연산이 이루어지면 기존 문자 배열은 버려지고 새로운 크기를 가지는 문자 배열이 새롭게 생성된다. 그러므로 초기 StringBuffer 생성시에 충분한 Capacity를 주어 불필요하게 크기가 확장되는 것을 방지할 수 있다.  
  
```
StringBuffer sb = new StringBuffer(512);
```
  
단, 너무 크게 잡아도 메모리 낭비 및 초기화 타임이 소요되므로 주의한다.  
  

#### **3. String Pool을 이용**

String 객체를 항상 생성하지 않고, Java Syntax 차원에서 제공하는 방법으로 문자열을 초기화한다.  
같은 문자열의 중복 생성을 방지할 수 있다.  
  
String Pool 포스팅편을 참고 : [http://www.xenomity.com/Java-String-Pool](http://www.xenomity.com/Java-String-Pool)

