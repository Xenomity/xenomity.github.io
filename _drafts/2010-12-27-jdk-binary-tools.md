---
title: "JDK Binary Tools"
date: 2010-12-27 08:26:28
tags: [java, jdk]
---

# JDK Binary Tools
HotSpot JDK 6 기준으로, 배포본에 포함되는 바이너리 도구들과 그에 상응하는 표준 또는 확장 패키지를 매핑한 내용이다. 보다시피 대부분의 도구들은 programmatically 사용이 가능하다.

| Binary    | API                   | Desc                          |
|-----------|-----------------------|-------------------------------|
| jps       | sun.tools.jps         | JVM Process List.             |
| jstat     | sun.tools.stat        | JVM Snapshot Tool.            |
| jstack    | sun.tools.stack       | JVM Thread Dump Tool.         |
| jmap      | sun.tools.map         | JVM Heap Dump Tool.           |
| jhat      | com.sun.tools.hat     | Binary Heap Analysis. (OQL)   |
| javac     | com.sun.tools.javac   | Compiler.                     |
| javap     | com.sun.tools.javap   | Bytecode Profiler.            |
| jconsole  |                       | Java Monitoring & Management. |
| jarsigner |                       | Jar Package Signer.           |
| javadoc   | com.sun.tools.javadoc | Java Documenting Utility.     |
| jar       | java.util.jar         | Jar Archive Utility.          |
