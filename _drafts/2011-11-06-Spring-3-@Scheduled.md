---
title: "Spring 3 @Scheduled"
tags: spring
date: 2011-11-06T15:45:47+09:00
---

Spring 3에서는 @Scheduled Annotation을 통한 스케줄링이 가능하다.  
다음은 cron-expression을 통한 스케줄링의 예이다.  
  
**예 1) TestJob.java**
```java
package com.xenomity.batch;
 
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;
 
@Component
public class TestJob {
 
    @Scheduled(cron = "0/10 * * * * ?")
    public void run() {
        System.out.println("메롱~~~~~~~~~~~~~~~~~~~~");
    }
 
}
```

Component-scan을 통한 자동 빈 등록을 위해, Job 클래스는 @Component Annotation으로 선언하였다.  
  
**예 2) applicationContext.xml**  
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:task="http://www.springframework.org/schema/task"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.0.xsd">
 
    <context:component-scan base-package="com.xenomity.batch" />
 
    ...
 
    <task:scheduler id="jobScheduler" pool-size="10" />
    <task:annotation-driven scheduler="jobScheduler" />
 
</beans>
```

Scheduler 선언은 spring-task namespace에 의해 정의되며, 스케줄러에 대한 Thread Pool 크기도 정의할 수 있다.  
  
**예 3) 결과**  
```
...  
  
정보: Server startup in 7113 ms  
메롱 ~~~~~~~~~~~~~~~~~~~~  
메롱 ~~~~~~~~~~~~~~~~~~~~  
```
  
  
@Scheduled Annotation을 사용하지 않고 xml 설정으로 추출할 경우, 다음과 같은 방법으로 가능하다.  
**예 4) applicationContext.xml**  
```xml
...
 
<task:scheduler id="jobScheduler" pool-size="10"/>
<task:scheduled-tasks scheduler="jobScheduler">
    <!-- fixed-rate, fixed-delay, cron-expression -->
    <task:scheduled ref="testJob" method="run" cron="0/10 * * * * ?"/> 
</task:scheduled-tasks>
  
<!-- job bean -->
<bean id="testJob" class="com.xenomity.batch.TestJob" />
 
...
```
  
