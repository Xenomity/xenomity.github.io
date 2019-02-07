---
title: "Java 6 Update 21의 jvm.dll 변동사항과 Eclipse"
tags: java
date: 2010-12-27T14:50:45+09:00
---

Oracle이 Java를 인수한 이후, "java.vm.vendor" System Property가 기존 "Sun Microsystems, Inc"에서 "Oracle Corporation"으로 변경되었다고 한다. 이로 인해 몇가지 Java 기반의 어플리케이션 동작에 문제가 발생하는데 대표적인 예가 Eclipse다.  
Eclipse Launcher는 "java.vm.vendor"가 "Sun Microsystems"이라는 문자열을 포함한 Windows 기반의 jvm 실행환경에서 자동으로 -XX:MaxPermSize=256m을 적용하는데, 위 변동사항으로 인해 Eclipse 기동 실패 혹은 OutOfMemoryError가 발생한다.  
이 경우에는 jvm argument로 XX:MaxPermSize를 임의로 넘겨주면 해결된다.  

  

[eclipse.ini]  
```
-vmargs
-XX:MaxPermSize=512m
```
