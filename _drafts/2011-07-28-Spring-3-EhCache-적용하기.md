---
title: "Spring 3 EhCache 적용하기"
tags: [ehcache, spring]
date: 2011-07-28T21:54:52+09:00
---

EhCache는 JVM 캐싱과 분산 캐싱(distributed/clustered)을 지원하는 대표적인 Java Cache Framework이다. 다양한 캐싱 모델을 지원하고 있으며, 물리디스크 또는 메모리를 캐싱 저장장치로 이용할 수 있다.  
  
EhCache를 Spring에 통합하고 이용하는 방법은 간단하다. 그냥 EhCache API를 이용할 수도 있지만, @Annotation 개발이 가능한 EhCache를 사용하기 위해서는 ehcache-spring-annotation.jar가 필요하다.  
현재일자 기준, 최신버전은 1.1.3이다.  
  
예 1) Maven pom.xml
```xml
<!-- EhCache -->
<dependency>
    <groupId>com.googlecode.ehcache-spring-annotations</groupId>
    <artifactId>ehcache-spring-annotations</artifactId>
    <version>1.1.3</version>
</dependency>
```

예 2) Spring applicationContext.xml  
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:ehcache="http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring
    http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring/ehcache-spring-1.1.xsd">
 
  <ehcache:annotation-driven cache-manager="ehCacheManager" />
 
  <bean id="ehCacheManager"
  class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
    <!-- EhCache Configurations File -->
    <property name="configLocation"
      value="classpath:/config/ehcache/ehcache-default.xml" />
  </bean>
</beans>
```
  
예 3) EhCache Configurations File  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
  updateCheck="false">
 
<cache name="testCache" eternal="false" maxElementsInMemory="100"
  overflowToDisk="false" diskPersistent="false" timeToIdleSeconds="0"
  timeToLiveSeconds="300" memoryStoreEvictionPolicy="LRU" />
 
</ehcache>
```

실제 여기서 다양한 캐싱 정책을 정의할 수 있으며, 자세한 내용은 [http://code.google.com/p/ehcache-spring-annotations/](http://code.google.com/p/ehcache-spring-annotations/) 을 참고한다.  
  
  
다음은 간단한 테스트 예제를 작성한 것이다.  
EhCache Configuration File에서 정의한 'testCache'를 적용하는 예이다.  
  
예 4) Spring Service  
```java
@Service
public class EmployeeServiceImpl implements EmployeeService {
 
    @Cacheable(cacheName = "testCache")
    @Override
    public EmployeeVO getEmployee(int id) {
        ...
    }
 
}
```
  
  
해당 서비스를 이용하는 액션을 호출하여 보면, 동일한 자원 요청에 대한 중복 요청이 감지되면 실제 서비스를 호출하지 않고 캐쉬된 결과만 리턴하는 것을 확인할 수 있다.  
  
결과 1) 최초 요청 발생시 로그  
```
[DEBUG] com.googlecode.ehcache.annotations.interceptor.EhCacheInterceptor - Generated key '1950589640028680' for invocation: ReflectiveMethodInvocation: public abstract com.xenomity.employee.model.EmployeeVO com.xenomity.employee.service.EmployeeService.getEmployee(int); target is of class [com.xenomity.employee.service.impl.EmployeeServiceImpl]
[DEBUG] java.sql.Connection - ooo Connection Opened
[DEBUG] java.sql.PreparedStatement - ==>  Executing: SELECT ID, NAME as name, AGE as age, MAN as man FROM TEST WHERE ID = ? 
[DEBUG] java.sql.PreparedStatement - ==> Parameters: 2(Integer)
[DEBUG] java.sql.ResultSet - <==    Columns: ID, NAME, AGE, MAN
[DEBUG] java.sql.ResultSet - <==        Row: 2, 순희, 19, 0
[DEBUG] com.xenomity.employee.service.impl.EmployeeServiceImpl - 'getEmployee(..)' Method Process Time : 24ms. 
```
  
결과 2) 중복 이상의 요청 발생시 로그  
```
[DEBUG] com.googlecode.ehcache.annotations.interceptor.EhCacheInterceptor - Generated key '1950589640028680' for invocation: ReflectiveMethodInvocation: public abstract com.xenomity.employee.model.EmployeeVO com.xenomity.employee.service.EmployeeService.getEmployee(int); target is of class [com.xenomity.employee.service.impl.EmployeeServiceImpl]
```

위의 결과를 보면, 두번째 요청부터는 Spring Service의 로그가 나타나지 않고 캐시 서비스가 Proxy로 응답한 것을 확인할 수 있다.

