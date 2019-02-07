---
title: "Spring Transction Attributes"
date: 2011-07-13 08:26:28
tags: [java, spring]
---

# Spring Transaction Attributes
Spring을 통한 트랜잭션 처리는 전략에 따라 트랜잭션 전파 범위와 격리 수준 등을 결정할 수 있다. 

```
<bean id="txManager"
    class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
</bean>
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="get" propagation="REQUIRED" read-only="true" />
        <tx:method name="*" propagation="REQUIRED" rollback-for="Exception" />
    </tx:attributes>
</tx:advice>
```
예 1. Transaction Manager AOP

```
@Transactional(readOnly = true,
               isolation = "READ_COMMITED",
               propagation = "REQUIRED",
               rollbackFor = Exception.class)
```
예 2. Spring JavaConfig

벤더에 따라 일부 지원되지 않거나 동작의 차이가 있지만, 보통 다음과 같은 명세를 따른다.

## Propagation
트랜잭션의 전파 범위와 참여 방식을 결정한다.

- REQUIRED (_* default_)
  - 기본 전략이며, 이미 실행중인 트랜잭션이 있다면 참여하고, 없다면 새롭게 트랜잭션을 시작한다.
  - 비지니스 계층의 컴포넌트 사이에 상호 호출이 불가피한 시스템에서 유용하게 사용된다.
  - 모든 `transactionManager`가 지원하는 전파 범위.
- SUPPORTS
  - 이미 실행중인 트랜잭션이 있다면 참여하고, 없다면 트랜잭션을 시작하지 않는다.
- MANDATORY
  - 이미 실행중인 트랜잭션이 있다면 참여하고, 없다면 새롭게 트랜잭션을 시작하지만 예외를 발생시킨다.
  - 독립적으로 트랜잭션이 발생하지 않아야 하거나, 새로운 트랜잭션 시작 시에 부수 처리가 필요한 경우 사용할 수 있다.
- REQUIRES_NEW
  - 이미 실행중인 트랜잭션이 있다면 일시 보류하고, 새롭게 트랜잭션을 시작한다.
  - 후속 트랜잭션이 우선 처리되어야 하거나 트랜잭션 처리에 제약이 필요한 경우 사용할 수 있다.
- NOT_SUPPORTED
  - 이미 실행중인 트랜잭션이 있다면 일시 보류하고, 트랜잭션을 사용하지 않는다.
- NEVER
  - 이미 실행중인 트랜잭션이 있다면 예외를 발생시키고, 트랜잭션을 사용하지 않는다.
  - 절대 트랜잭션을 사용하면 안 되는 경우 사용할 수 있다.
- NESTED
  - 이미 실행중인 트랜잭션이 있다면 중첩(하위) 트랜잭션을 시작하고, 없다면 새롭게 트랜잭션을 시작한다.
  - 중첩 트랜잭션은 상위 트랜잭션의 커밋과 롤백에 영향을 받지만, 자신의 트랜잭션은 고립되어 실행된다.

## Isolation
트랜잭션의 격리 수준을 결정한다.

- DEFAULT
  - 데이터베이스에서 제공하는 기본 격리 수준을 따른다.
- READ_COMMITTED
  - 일반적인 데이터베이스에서 제공하는 기본 격리 수준이다.
  - 진행중인 트랜잭션이 커밋하지 않은 데이터는 다른 트랜잭션이 읽을 수 없다.
  - 즉, 다른 트랜잭션이 접근하여 변경 이전 시점의 데이터를 읽게 되는 경우, 재-조회가 필요할 수 있다.
- READ_UNCOMMITTED
  - 가장 낮은 격리 수준으로, 빠른 데이터 영속화가 필요할 경우 사용한다.
  - 다른 트랜잭션에 현재 트랜잭션의 데이터 변경 내용이 그대로 노출되므로, data consistency가 보장되지 않는다.
- REPEATABLE_READ
  - 진행중인 트랜잭션에 변경 수준의 `transaction lock`을 부여한다.
  - 현재 트랜잭션이 데이터를 조회하는 경우, 다른 트랜잭션은 동일 데이터를 변경할 수 없다.
- SERIALIZABLE
  - 순차적인 트랜잭션을 통해 직렬화된 처리를 보장하는 격리 수준이다.
  - 각 트랜잭션은 동시/동일 테이블에 접근할 수 없으므로 가장 안전하고 강력한 격리 수준이지만, 성능이 매우 떨어지기 때문에 거의 사용되지 않는다.

## Rollback For
트랜잭션이 진행되는 도중에 특정 예외가 발생하는 경우, 스프링 컨테이너는 해당 트랜잭션을 롤백하고 예외를 던진다.

## Read Only
성능을 최적화하거나 데이터의 변경을 제한하기 위한 목적으로 특정 트랜잭션을 읽기 전용 트랜잭션으로 선언할 수 있다.
