---
title: "JNA (Java Native Access)"
tags: java
date: 2012-02-06T20:50:53+09:00
---

JNA는 기존 JNI(Java Native Interface)의 복잡한 구현 방식을 단축하고자 나온 OpenSource Library이다.  
  
- [https://github.com/twall/jna](https://github.com/twall/jna)  
  
JNA를 이용하면 JNI의 native library와의 연결을 위한 javah header파일 생성, native source 파일 생성, 컴파일 등의 과정이 불필요하며, 이미 만들어진 native library의 재사용이 가능하게 된다. 말 그대로 native access임..!  
  
**예 1) JNI 개발 과정**
```
1) Native Method 선언된 Java Class 코딩  
2) Java Class 컴파일  
3) javah를 통한 C Header파일 생성  
4) C 소스 코딩  
5) C 소스 컴파일  
6) Execute Java Class  
```
  
**예 2) JNA 개발 과정**  
```
1) Native Method 선언된 Java Class 코딩  
2) Java Class 컴파일  
3) Execute Java Class
```

참고로 최근에 많은 OpenSource들이 JNA를 이용하여 성능을 향상시키는 방식를 적용하고 있으며, 무엇보다도 지저분한 h,c 파일들이 프로젝트 내에서 사라지게 되어 매우 깔끔해지고 관리도 수월하다. ;)  
  
다음은 JNA를 통해 test.so의 foo라는 함수를 호출하는 간단한 예제를 만들어 보았다.  
  
  

## 1. TestLibrary Interface
```java
import com.sun.jna.Library;
import com.sun.jna.Native;
 
/**
 * Test Library Interface
 *
 * @Author Xenomity™ a.k.a P-slinc' (이승한)
 */
public interface TestLibrary extends Library {
    TestLibrary LIBRARY = (TestLibrary) Native.loadLibrary(("test.so"), TestLibrary.class);
 
    /**
     * foo method
     */
    void foo();  // native method
}
```

## 2. Test Result
```
TestLibrary.LIBRARY.foo();

...  
  
[RESULT] foo function executed~~!.  
```

ps. Native Code를 사용하는 빈도가 높다면 Java의 플랫폼 독립적인 장점을 떨어뜨리고 원론적으로 언어 선택이 잘못된 설계라고도 할 수 있다. 다른 표준화된 통신(or interface) 기술을 적용하는 방향으로 전환할 필요가 있다.

