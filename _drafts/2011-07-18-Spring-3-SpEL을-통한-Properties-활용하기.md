---
title: "Spring 3 SpEL을 통한 Properties 활용하기"
tags: spring
date: 2011-07-18T05:01:00+09:00
---

기존 스프링 2.5.x 버전의 property-placeholder와 EL을 통한 properties 활용이 개인적으로는 더 친숙하고 편했던것 같지만... 없는 프로퍼티 값을 요청할 경우 그대로 찍히는 EL 표현식때문에 일일이 체크하기에 좀 불편한감도 없지 않았다.;  
  
대신 Spring 3에 추가된 SpEL 표현식과 `PropertiesUtil`을 이용하면 어느정도 저런 불편함을 커버할 수 있다.   
  
  
설정 예1) Properties File

# config.properties  
```
auth.id=xxx  
auth.pw=yyy  
```
  
설정 예2) applicationContext.xml  
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:p="http://www.springframework.org/schema/p"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:util="http://www.springframework.org/schema/util"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.0.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">
 
...
 
<util:properties id="config" location="classpath:/config/config.properties" />
```

util 태그를 사용하기 위해서는 spring-util 스키마가 선언되어 있어야 한다.  
  
  
활용 예1) applicationContext.xml  
```xml
<!-- SpEL을 통한 활용 -->
<bean id="authorityLoader" class="...">
    <property name="id" value="#{config['auth.id']}"/>
    <property name="pw" value="#{config['auth.pw']}"/>
</bean>
```
  
활용 예2) Java  
```java
@Value("#{config['auth.id']}") String id;
@Value("#{config['auth.pw']}") String password;
 
...
 
logger.info("당신의 ID는 " + id + "입니다.");
```
  
활용 예3) JSP  
```jsp
<%@ taglib uri="http://www.springframework.org/tags" prefix="spring"%>
 
...
  
<spring:eval expression="@config['auth.id']" />
```
  
