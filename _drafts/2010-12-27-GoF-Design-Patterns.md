---
title: "GoF Design Patterns"
tags: [design-pattern, oop]
date: 2010-12-27T15:08:32+09:00
---

GoF(Gang of Four)에서는 23가지 객체지향 설계 패턴을 3가지 유형으로 분류한다.

#### **1. Creational Pattern (생성패턴)**
- 객체 생성에 관련한 패턴들.  
- 최대한 느슨한 연결로 객체생성의 유연성을 높힌다.  
- 코드의 유지보수가 용이.  
  
  1) Abstract Factory : 구체적인 클래스를 정하지 않고, 상호 관련있는 객체들의 집합을 생성하는 방법을 정의.  
  2) Prototype : 객체를 복사하여 새로운 객체를 생성.  
  3) Singleton : 특정 객체가 단 하나만 생성됨을 보장하며, 그 객체에 대한 전역접근이 가능하  
도록 정의.  
  4) Factory Method : 생성할 객체를 하위클래스에서 결정하도록 정의.  
  5) Builder : 객체 생성부와 표현부를 분리하여 서로 다른 표현 객체들이 동일한 객체 생성부를 이용할 수 있도록 정의.  
  

#### **2. Structural Pattern (구조패턴)**
- 객체간의 구조에 관련한 패턴들.  
  
  1) Adapter(Wrapper) : 기존 인터페이스를 다른 인터페이스로 변환. 상호 호환성 제공.  
  2) Bridge : 객체 구현부와 추상부를 분리하여 설계함으로써, 두 부분이 독립적으로 변경될 수 있는 방법을 정의.  
  3) Composite : 부분-전체(Part-Hole Hierachy) 구조를 표현하기 위한 트리 구조로 구성된 객체 설계. 부분부, 전체부를 동일시화하는 방법을 정의.  
  4) Decorator : 특정 객체에 대한 책임 사항들(Responsibilities)을 동적으로 추가. 기능 확장을 위한 방법을 정의.  
  5) Flyweight : 객체들을 Caching하여 효율적인 공유 방법을 정의.  
  6) Facade : 여러 인터페이스들에 대한 통합된 상위 인터페이스(High-Level Interface)를 정의.  
  7) Proxy : 가벼운 선처리 혹은 위임처리를 위한 대리자(surrogate) 구현을 정의.  
  

#### **3. Behavioral Pattern (행동패턴)**
- 반복적으로 사용되는 객체들의 상호작용에 관련한 패턴들.  
  
  1) Chain of Responsibility : 서비스의 체인화로 서비스 요청자와 제공자간의 결합도를 줄임. (De-coupling)  
  2) Command : 명령을 객체화시킴으로써, 이전/다음 등의 이력관리 및 복구를 가능하도록 정의.  
  3) Interpreter : 자체 언어/문법을 정의.  
  4) Iterator : 자료 요소들의 순차적 접근방법을 정의.  
  5) Mediator : 객체간의 상호 작용을 중재하는 방법을 정의.  
  6) Memento : 객체의 상태를 외부 객체화한다. 저장 및 복구를 위한 방법을 정의.  
  7) Observer : 특정 객체의 변동사항을 다른 객체들에게 통지하는 방법을 정의.  
  8) State : 객체의 상태정보 변동에 따른 행동방법을 변경하는 방법을 정의.  
  9) Strategy : 알고리즘 구현부를 동적으로 변경하는 방법을 정의.  
  10) Template Method : 로직 구현부를 하위클래스로 위임하여 동일 인터페이스의 다형성 정의.  
  11) Visitor : 자료구조 내의 객체들에게 특정 연산을 수행하는 방법을 정의.  
  

![Design Pattern Relationship](/assets/image/design-pattern-relationship.jpg)
 

![Design Pattern Reference 1](/assets/image/design-pattern-reference-1.gif)


![Design Pattern Reference 2](/assets/image/design-pattern-reference-2.gif)

