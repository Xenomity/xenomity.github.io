---
title: "Java SE 8 New Features"
tags: java
date: 2014-02-04T18:52:36+09:00
---

Java SE 8(JSR-337)의 새로운 특징들에 대해 간략히 정리한다.

Java Standard Edition 8 버전의 변동사항으로는 다음과 같은 JSR(Java Specification Request)들이 반영된다.

 - JSR 308: Annotations on Types (\* new)

 - JSR 310: Date and Time API (\* new)

 - JSR 335: Lambda Expressions (\* new)

 - JSR 114: JDBC Rowsets

 - JSR 160: JMX Remote API

 - JSR 199: Java Compiler API

 - JSR 173: Streaming API for XML

 - JSR 206: Java API for XML Processing

 - JSR 221: JDBC 4.0

 - JSR 269: Pluggable Annotation-Processing API

#### **- Lambda Expressions**

Closure와 함수 프로그래밍을 가능하게 하는 람다 표현식이 추가되었다. 람다 표현식은 익명 함수 형식의 프로그래밍 모델을 제공한다.

    public interface DoStuff { boolean isGood(int x); } void doSomething(DoStuff d) { if (d.isGood(myVariable)) { ... } } doSomething(answer -\> answer == 42);

- 예 1.1. Inner Class의 람다 표현식화 -

#### **- Extension Methods**

다중 상속에 대한 프로그래밍 모델을 제공한다. 실제로는 'interface default method'의 구현을 통한 compiler trick으로 다중 상속의 효과를 제공하므로, '확장 메서드(extension method)'로 부른다.

#### **- Annotations on Types**

Annotation을 class나 method, variable이 아닌, method의 parameter와 같은 type에 적용 가능하도록 한다.

    public void process(@NotNull List data) { … } 

- 예 3.1. @NotNull Annotation Processing -

#### **- Generalized Target-Type Inference**

Generic의 사용성이 개선되었다.

    class List\<E\> { static \<Z\> List\<Z\> nil() { ... }; static \<Z\> List\<Z\> cons(Z head, List\<Z\> tail) { ... }; E head() { ... } } List\<String\> ls = List.nil(); // Inferred correctly List.cons(42, List.nil()); // Error: expected List\<Integer\>, found List\<Object\>

- 예 4.1. 개선된 Generic -

#### **- Access To Parameter Names At Runtime**

메서드나 생성자의 파라메터 명을 runtime 시점에 획득할 수 있는 reflection mechanism을 제공한다. 이를 통해 불필요한 어노테이션의 사용을 줄일 수 있으며 코드의 가독성을 높이고, 코드 자동 생성과 같은 template engine을 개발 환경으로 쉽게 통합 할 수 있다.

#### **- Pluggable Annotation-Processing API**

APT(Java Annotation Processing Tool) 도구나 연관 API들은 전부 JSR-269 구현체로 통합되었다.

#### **- DocTree API**

javadoc comment의 문법 요소 접근 방법을 제공한다.

#### **- DocLint Tool**

DocTree API를 통하여 javadoc comment의 기본적인 오류를 식별한다.

#### **- Javadoc support in javax.tools**

javax.tools API를 통해 javadoc 도구를 호출할 수 있다.

#### **- Bulk Data Operations For Collections**

Java를 위한 Filter, Map, Reduce를 제공한다. 람다 구문으로 표현되는 직렬/병렬 구현체들이 있으며, 병렬 구현체의 경우 Fork-Join 프레임워크에서 빌드될 수 있다.

#### **- Parallel Array Sorting**

java.util.Arrays.parallelSort(...) API가 추가되었다. Dual-core 시스템에서, 기존 정렬에 비해 최소 30% 이상의 성능 향상을 기대할 수 있다.

#### **- Date And Time APIs**

새로운 Date/Time API가 추가되었다. 이들은 ISO-8601, CLDR, BCP47 표준을 준수하며, UTC 연결을 통한 명시적인 time-scale 기반으로 동작한다.

#### **- JDBC 4.2**

사용성과 이식성 향상을 위한 업데이트가 이루어졌다.

#### **- Base64 Encoding and Decoding**

sun.misc.BASE64Encoder/BASE64Decoder는 새롭게 표준화되어 자바 표준 패키지에 포함되었다. (java.util.Base64.Encoder/Decoder)

#### **- Reduced core-library memory usage**

객체 및 internal table 등의 사이즈 감소.

#### **- Statically Linked JNI Libraries**

Embedded 어플리케이션을 위한 정적 JNI 라이브러리 링크 제공.

#### **- Locale Data Packing**

로케일 데이터 파일(LDML format) 생성을 위한 도구를 제공한다. CLDR(Unicode Common Locale Data Repository)를 지원한다.

#### **- Unicode 6.2**

유니코드 6.2 지원.

#### **- Enhanced Certificate Revocation-Checking API**

java.security.cert API는 인증 서버로의 연결 실패시에 발생하는 fatal error를 사전에 방지하기 위한 RevocationChecker, RevocationParameters와 같은 추가 클래스를 제공한다.

#### **- HTTP URL Permissions**

자원 접근에 대한 권한 제어 모델을 제공한다.

#### **- Launch JavaFX Applications**

Java 명령행 런처를 향상시켜, JavaFX 어플리케이션을 바로 실행할 수 있도록 지원한다.

#### **- Modularisation**

Java 7에서 결국 빠졌던 Java Modulesystem(Jigsaw Project)가 포함되었다. 모듈 지정과 버전 관리 등의 기능을 제공한다. (그러나 최종 릴리즈에서는 결국 다시 빠졌다는..;;!!!)

#### **- Stripped Implementations**

어플리케이션이 요구하는 최소한의 라이브러리만으로 구성된 경량 JRE를 번들할 수 있다. 번들된 JRE는 다른 Java 어플리케이션에서 사용되지 않는다.

#### **- Nashorn JavaScript Engine**

Nashorn JavaScript Engine은 고성능의 경량 JavaScript Engine으로서, ECMAScript-262 Edition 5.1 language specification을 준수한다. 이 엔진은 JRE에 통합되어. javax.script 확장 패키지 APIs 및 'jjs' 명령어 도구를 제공한다.

#### **- Remove The Permanent Generation**

'PermGen' 메모리 영역은 사라진다. Interned String이나 Class Metadata, static variables는 기존 JVM의 PermGen 영역에서 Heap이나 native 메모리 영역으로 이전된다. 그리고 'Metaspace'라는 새로운 메모리 영역이 추가되었다.

(\* 관련된 내용은 따로 자세히 포스팅 할 예정.)

#### **- Small VM**

Embedded 환경을 위한 초경량의 VM을 구성할 수 있다.

#### **- The JDK**

'./configure' 스타일의 빌드 설정을 제공한다. 또한 'javac'는 빌드시, 사용가능한 모든 코어를 사용하며 native method에 대한 header 파일들을 자동으로 생성하고, 더이상 필요하지 않는 class와 header 파일들을 제거하므로서 빠른 빌드 속도를 보장한다.

 

더 자세한 내용은 [jdk8.java.net](http://jdk8.java.net/)을 참고한다.

 

