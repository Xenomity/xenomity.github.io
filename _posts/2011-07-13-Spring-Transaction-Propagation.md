---
title: "Spring Transaction Propagation"
tags: spring
date: 2011-07-13T04:42:00+09:00
---

트랜잭션 전파 방법에 관한 속성을 정의한다.  
  
예) applicationContext.xml
```xml
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource" />
</bean>
<tx:advice id="txAdvice" transaction-manager="txManager">
  <tx:attributes>
    <tx:method name="get" propagation="REQUIRED" read-only="true" />
    <tx:method name="*" propagation="REQUIRED" rollback-for="Exception" />
  </tx:attributes>
</tx:advice>
```
  
### Propagation 종류

| Propagation | Description |
|-------------|-------------|
| **REQUIRED** (default) | 이미 시작된 트랜잭션이 있는 경우 함께 묶이며, 없는 경우 새로 시작. |
| **SUPPORTS** | 이미 시작된 트랜잭션이 있는 경우 함께 묶이며, 없는 경우 트랜잭션 없이 진행. |
| **MANDATORY** | 이미 시작된 트랜잭션이 있는 경우 함께 묶이며, 없는 경우 새로 시작하지만 Exception 발생. |
| **REQUIRES_NEW** | 이미 시작된 트랜잭션이 있는 경우 대기시키고, 새로운 트랜잭션 시작. |
| **NOT_SUPPORTED** | 이미 시작된 트랜잭션이 있는 경우 대기시키고, 트랜잭션을 사용하지 않음. |
| **NEVER** | 이미 시작된 트랜잭션이 있는 경우 Exception을 발생시키며, 트랜잭션을 사용하지 않도록 제한. |
| **NESTED** | 이미 시작된 트랜잭션이 있는 경우, 종속적인 자식 트랜잭션 시작. 부모 트랜잭션의 영향을 받음. |

  
  
