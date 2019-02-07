---
title: "Synchronized의 동작 원리"
tags: java
date: 2012-11-05T17:16:44+09:00
---

Java에서 동기화는 synchronized keyword를 통해 이루어지며, 실제로는 OS에서 제공되는 쓰레드의 구현 기능을 사용하게 된다. 여기서 동기화란, 공유 리소스에 대한 멀티 쓰레드 환경에서의 접근 순서를 순차적으로 부여함을 의미하며, 순차 처리에 의한 공유 리소스의 일관성을 보장하게 된다.

이미 Java의 Thread에 대해 알고 있다고 가정하고 Object lock에 대한 설명은 제외한다.

실제로 동기화 매커니즘은 lock의 획득과 대기(wait)의 반복이라고 봐도 무방하다.

#### **1. Synchronized Method**

synchronized method를 통한 동기화는 자기 자신의 인스턴스에 대해 lock object로 설정하게 된다. 따라서 'synchronized (this)'와 같다고 생각하면 되며, 클래스 내부의 다른 synchronized method에 대해서도 동일한 locking이 적용된다.

예 1) Synchronized Method

    public class SynchronizedTest { public synchronized void a() { ... } public synchronized void b() { ... } }

여기서는 SynchronizedTest 클래스의 인스턴스가 a(), b() 두 메서드의 lock 객체가 된다. 따라서 특정 쓰레드가 SynchronizedTest 클래스의 a() 메서드를 수행중이라면, 다른 쓰레드가 a()가 아닌 b() 메서드를 호출하여도, a() 메서드의 수행이 끝날때까지 대기 상태가 된다. 동기화의 lock 객체가 동일하기 때문이다.

결국 예제 1 코드는 다음과 같이 작성될 수 있다.

    public class SynchronizedTest { public void a() { synchronized (this) { ... } } public void b() { synchronized (this) { ... } } }

#### **2. Static Synchronized Method**

예 2) Static Synchronized Method

    public class StaticSynchronizedTest { public static synchronized void a() { ... } public static synchronized void b() { ... } }

보통 static 필드의 동기화 목적으로 static synchronized 구문을 사용하지만, 실제로는 하나의 클래스 내부의 synchronized method 단위로 lock 객체는 달라지게 된다. 그 이유는 lock 객체가 해당 클래스의 인스턴스가 아닌, 해당 클래스의 클래스 객체이기 때문이다.

위 코드는 다음과 같이 재작성 될 수 있다.

    public class StaticSynchronizedTest { public static void a() { synchronized (StaticSynchronizedTest.class) { ... } } public static void b() { synchronized (StaticSynchronizedTest.class) { ... } } }

고로 a()와 b()는 서로 다른 lock 객체를 가지게 되며, 하나의 메서드 단위의 동기화가 보장된다. 즉, a()가 수행되는 동안 다른 쓰레드에 의해 b()가 수행될 수 있지만, a()의 수행이 끝나기 전까지는 다른 쓰레드에 의해 a() 호출은 대기하게 된다.

#### **3. Synchronized Block**

좀 더 정교한 동기화를 구현하기 위해 synchronized block 구문을 사용한다. 대부분의 Code Inspection Rule에서는 메서드 내부의 최소화된 로직만 block으로 감싸는 성능 최적화 목적으로 권장하고 있지만, 서로 다른 lock 객체를 통하여 다양한 동기화를 보장하도록 하는데 그 목적이 있다.

예 3) Synchronized Block

    public class SynchronizedBlockTest { private Object \_lock1 = new Object(); private Object \_lock2 = new Object(); private static Object \_lock3 = new Object(); public void a() { synchronized (\_lock1) { ... } } public void b() { synchronized (\_lock2) { ... } } public void c() { synchronized (\_lock2) { ... } } public static void d() { synchronized (\_lock3) { ... } } }

여기서 동기화 단위는 a(), b() + c(), d()가 된다.

