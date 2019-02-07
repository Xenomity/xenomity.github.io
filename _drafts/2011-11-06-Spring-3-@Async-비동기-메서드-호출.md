---
title: "Spring 3 @Async 비동기 메서드 호출"
tags: spring
date: 2011-11-06T17:03:40+09:00
---

Spring 3 @Async Annotation을 통해 비동기 호출 로직을 쉽게 구현할 수 있다. 실제로 @Async Method는 ThreadExecutor를 통해 별도 Thread로 동작하게 된다.  
  
  
**예 1) Async Method**

    @Async public void test1() { ... } @Async public Future\<TestVO\> test2() { ... return new AsyncResult\<TestVO\>(testVO); }

  
@Async 메서드의 반환 타입은 void와 java.util.concurrent.Future만 가능하다.  
  
  
**예 2) applicationContext.xml**  

    ... \<task:annotation-driven executor=”executor” scheduler=”scheduler” /\> \<task:executor id=”executor” /\> \<task:scheduler id=”scheduler” /\> ...

  
  
**예 3) Test**  

    testService.test1(); Future\<TestVO\> asyncResult = testService.test2(); testService.other(); ... logger.debug(asyncResult.get()); // 비동기 메서드 결과

  
실제 테스트를 해보면, test1(), test2() 메서드의 로직이 끝나기도 전에, other() 메서드가 호출되는 것을 확인할 수 있다.

