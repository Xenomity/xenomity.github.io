---
title: "Summary Java Agent (Bytecode Instrumentation)"
tags: java
date: 2012-10-03T19:28:00+09:00
---

## Overview
Java SE 5 에서 `Bytecode Instrumentation`의 범주로 새롭게 소개된 'Java Agent' 명세에 대하여 간단히 소개하고자 한다. (* 일반적인 Agent 의미와 혼용되지 않는다.)

JVM은 전통적으로 특정 작업을 가로채기 할 목적으로 JVM Agent를 지정할 수 있는 방법을 제시해 왔지만 C/C++과 같은 native 언어 수준에서만 구현이 가능했다. 그러던 중에 Java SE 5부터 Java 언어로 구현 가능한 agent interface를 제공한다는 점은 기쁜 소식 중 하나가 아닐 수 없다. :) 이와 관련하여 추가된 JVM start-up argument로는 아래와 같이 java 명령어를 통해서도 확인할 수 있다.

```
-agentlib:<libname>[=<options>] <libname> 고유 에이전트 라이브러리를 로드합니다(예: -agentlib:hprof). -agentlib:jdwp=help 및 -agentlib:hprof=help도 참조하십시오.
-agentpath:<pathname>[=<options>] 전체 경로명을 사용하여 고유 에이전트 라이브러리를 로드합니다.
-javaagent:<jarpath>[=<options>] Java 프로그래밍 언어 에이전트를 로드합니다. java.lang.instrument를 참조하십시오.
```
- 예 1. `java -help` commands 중...

- `agentlib/agentpath` 옵션은 Java 1.5 이전 버전에서 제공되는 JVM Profiler Interface (이하 JVM PI)로 구현된 native agent 또는 Java 1.5에서 추가된 JVM Tool Interface(이하 JVM TI)로 구현된 native agent를 등록한다.
- 여기서 javaagent 옵션은 Java 언어로 구현된 agent를 등록한다.

-Java Agent-는 Java로 구현하기에 저수준의 프로파일링은 어려울 수 있다. 따라서 Java 5는 위에서 언급한 JVM TI/JVM PI로 구현된 에이전트와 Java Agent를 병행 지원한다. 이는 JVM 프로파일링과 같은 저수준 hook을 활용하기에 JVM TI를 C/C++로 구현한 native agent가 알맞고, JVM 위에서 통제 가능한 부문(e.g. redefine bytecode, class/object profiling, etc...)을 통한 에이전트 개발은 Java Agent가 적합하다는 의미이기도 하다. 다음은 Java Agent에 대하여 간략히 정리한 내용이다.

## What are Java(JVM) Agent?
1. JVM Agent는 JVM에서 동작하는 Java 어플리케이션으로 JVM의 다양한 이벤트를 전달받거나 정보 질의, 바이트 코드 제어 등을 특정 API(_Instrumentation API -java.lang.instrument-_)를 통하여 수행할 수 있다.
2. 보통 개발 도구 또는 모니터링 도구 개발에 응용된다.
3. 바이트 코드 변조를 통한 개발의 편의성을 제공하는 AspectJ의 LTW(Load Time Weaver)나 Lombok과 같은 오픈소스가 대표적인 Java Agent의 활용 예이다.

## Features
1. Agent는 지정된 JVM의 실행 가능한 최초 진입점인 'main' 메서드를 가로채기 할 수 있다.
2. 지정된 JVM에서 실행된다.
3. 지정된 JVM의 동일한 System Class Loader 내에서 로드된다.
4. 지정된 JVM의 Security Policy 및 Context의 영향을 받는다.
5. 실행시간에 동적으로 bytecode를 조작할 수 있다.

## Method Signature
```java
public static void premain(String agentArgs, Instrumentation inst);
public static void premain(String agentArgs);
```

- 예 2. `premain` Method Signature

Agent의 단일 진입점은 위와 같이 'premain' 메서드를 구현하면 되며 바이트 코드를 포함한 추가적인 정보 수집 도구로 Instrumentation 인터페이스를 제공받을 수 있다. Instrumentation 패키지에 대한 자세한 내용은 [javadoc](http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/instrument/package-summary.html)을 참고한다.

```java
public class TestAgent {
    public static void premain(String agentArgs) {
        System.out.println("agent loaded.");
    }
}
```
- 예 3. 간단한 Java Agent 코드

## MANIFEST
Java Agent 패키지는 일반적인 executable-jar와 생성 방식이 동일하지만 MANIFEST(_META-INF/MANIFEST.MF_) 명세는 약간의 차이가 있다.
```
Manifest-Version: 1.0
Premain-Class: com.xenomity.agent.TestJavaAgent
Boot-Class-Path: ./asm.jar
Can-Redefine-Classes: true
Created-By: Xenomity
```
- 예 4. MANIFEST.MF

-예 4-에서 보듯이 `Premain-Class`, `Boot-Class-Path`, `Can-Redefine-Classes`는 Agent 전용 속성으로 각 의미하는 바를 아래 표에 나타내었다.


| Property | Value |
|-|-|
| **Premain-Class** | 'premain' 메서드가 존재하는 에이전트 클래스 명시. |
| **Boot-Class-Path** | Bootstrap ClassLoader로 로딩될 에이전트가 사용하는 외부 라이브러리를 명시. (만약 Bootstrap ClassLoader에 의해 로딩되는 자바 표준 패키지(_rt.jar_)의 바이트 코드 재정의를 허용한다면, 클래스 로더의 수행 순서에 따른 가시성 원칙에 따라 에이전트 패키지지 또한 이곳에 명시되어야 한다.) |
| **Can-Redefine-Classes** | 실행시점에 바이트 코드의 재정의 허용 여부 명시. |
- 표 1. MANIFEST Properties

## Run & Test
TestAgent 클래스가 패키징된 jar 파일을 `test-agent.jar`라 하고, 실제로 실행될 클래스 -_Java SE 환경의 경우 `main` 메서드가 포함된_- 가 TestMain 클래스라면 다음과 같이 실행하고 결과를 확인할 수 있다.
```
> java -javaagent test-agent.jar TestMain
 
---------------
agent loaded.
```

## Conclusion
본 포스팅에서는 아주 간략한 예제만을 다뤘지만 실제로 Java Agent를 통하여 해결할 수 있는 문제의 범위는 굉장히 넓다. JMX와 더불어 막강한 모니터링 시스템을 구축하는 것도 시간 문제..

## References
- See [http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/instrument/package-summary.html](http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/instrument/package-summary.html)

