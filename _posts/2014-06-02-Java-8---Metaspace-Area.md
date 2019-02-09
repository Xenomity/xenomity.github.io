---
title: "Java 8 - Metaspace Area"
date: 2014-06-02 08:26:28
tags: [java, jvm]
---

## PermGen 영역이 사라지다
**Permanent Generation(이하 PermGen)** 영역은 JVM Heap 영역의 일부로 클래스의 메타데이터를 저장하는 공간이었다. Oracle HotSpot JVM은 Java 7부터 이미 PermGen 영역을 제거하기 위한 사전 작업으로 일부 정보들(*e.g. Symbols, Interned String, Static class variables*)을 Heap 영역으로 이전시킨 바 있다. 그리고 Java 8에 이르러 Heap 영역의 PermGen은 완전히 제거되고, native 메모리를 할당하는 새로운 **Metaspace** 영역이 추가되었고 클래스 메타데이터는 전부 이곳으로 이동하게 되었다. 따라서 PermGen과 관련된 기존 JVM 비표준 System Properties(*e.g. PermSize, MaxPermSize*)는 더 이상 유효하지 않으며, `java.lang.OutOfMemoryError: PermGen Space`와 같은 오류를 볼 수 없게 되었다.

## 왜 PermGen을 제거했을까?
여러가지 이유가 있겠지만 대표적인 내용만 나열해 보면 다음과 같다.
- 시작 시점의 고정 크기로 인한 `OutOfMemoryError` 발생의 위험. (*e.g. -XX:MaxPermSize=64M*)
- 객체 스캐닝 시간이 GC 성능 지연의 꽤 많은 시간을 차지.

## Metaspace 영역의 특징
- No GC scan or compaction.
- Native 메모리를 사용하므로 OS에서 사용 가능한 메모리 크기만큼 할당이 가능하다.
- PermGen 영역과 반대로 실행 시점에 가변적인 크기 할당과 변경이 이루어진다.
- 새로운 비표준 속성인 `MaxMetaSpaceSize`를 통해 Metaspace 영역의 최대 크기를 지정할 수 있다.
- GC 성능 향상. (*See [Improved GC Performance](http://java-latte.blogspot.kr/2014/03/metaspace-in-java-8.html)*)
- 단편화의 최소화.

## Conclusion
JVM 업그레이드를 고려하는 경우, 확인해야 할 몇 가지 사항이 있다. 첫째, PermGen 영역의 일부 데이터가 Heap 영역으로 이동하게 되므로 HotSpot 8 에서는 Heap 메모리 크기를 증가시켜야 할 수 있다. 둘째, 기존에 사용중인 프로파일러나 모니터링 도구가 PermGen 영역을 감시하고 있다면, 새로운 JVM에서는 동작하지 않는다. Metaspace 영역을 모니터링 할 수 있는 새로운 도구로 정비해야 할 필요가 있다.

## References
- Metaspace in Java 8: [http://java-latte.blogspot.kr/2014/03/metaspace-in-java-8.html](http://java-latte.blogspot.kr/2014/03/metaspace-in-java-8.html)
- Java8: From PermGen to Metaspace: [http://javaeesupportpatterns.blogspot.ie/2013/02/java-8-from-permgen-to-metaspace.html](http://javaeesupportpatterns.blogspot.ie/2013/02/java-8-from-permgen-to-metaspace.html)
