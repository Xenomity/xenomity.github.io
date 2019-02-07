---
title: "Core J2EE Patterns  - 3. Integration Tier"
tags: [architecture, design-pattern, jee, oop]
date: 2012-07-01T20:35:00+09:00
---

#### **1. Data Access Object**

데이터 액세스 및 조작에 대해 캡슐화하는 디자인 패턴.

클라이언트에게 데이터 액세스 로직 및 자원에 대한 접근 방법을 숨기고, 데이터 스토리지의 종류와 상관없이 투명한 인터페이스를 제공한다. JDBC나 SQL Mapper류를 사용하는 경우 대표적으로 쓰이는 패턴이며, 클라이언트는 보통 Business Object가 된다.

(\* 대표적인 SQL Mapper로는 MyBatis나 Oracle SQLJ 등이 있음)

[##\_1C|cfile23.uf.2445675051D167D7318C3B.gif|width="532" height="265" filename="DAOMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile29.uf.27486E5051D167D72BB74C.gif|width="590" height="547" filename="DAOMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **2. Service Activator**

서비스를 비동기(async) 방식으로 호출하는 JMS 디자인 패턴.

MOM을 통해 요청 메세지를 전달하면, Service Activator는 전달받은 메세지를 타겟 비지니스 서비스가 사용할 수 있도록 parsing하고, 비지니스 처리를 위임한다. 독립적인 JMS Listener를 구현할 수 있다는 것이 장점.

(\* MOM: 'Message Oriented Middleware')

[##\_1C|cfile28.uf.2230724C51D1682431AA0F.gif|width="590" height="283" filename="SAMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile29.uf.2266A34C51D1682512548A.gif|width="590" height="356" filename="SAMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **3. Domain Store**

비지니스 로직에서 데이터 영속 로직을 분리하기 위한 디자인 패턴.

Businee Object에서 데이터 영속화 로직을 분리함으로서 재사용성 및 테스트의 편의성이 높아진다. 보통 JDO나 JPA Spec 구현체를 도입하여 사용한다.

(\* Data Access Object는 SQL Mapper와 같이 데이터 스토리지에 대한 오퍼레이션을 수행하는 adapter 역할을 하지만, Domain Store는 Entity Bean처럼 'Persistence Object' 역할을 함)

[##\_1C|cfile2.uf.216F464B51D1688F31DBF2.gif|width="590" height="304" filename="DSMainClass1.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile24.uf.0269A14B51D1688F34E426.gif|width="590" height="427" filename="DSMainSeq3.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **4. Web Service Broker**

XML 기술자와 웹 프로토콜을 통하여 서비스 접근을 제공하는 디자인 패턴.

Web Service Broker는 다양한 프로토콜에 의존적인 요청 처리 로직을 캡슐화하고, Endpoint Processor를 통해 클라이언트로부터 단순하고 표준화된 리소스 접근 방법을 제공한다.

(\* API Service에 많이 사용되는 패턴)

(\* Spring Web Services에 이 패턴이 잘 드러나 있음)

[##\_1C|cfile24.uf.2577604951D1692804712D.gif|width="590" height="195" filename="WSBMainClass1.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile26.uf.216CD64951D16928090AEA.gif|width="590" height="341" filename="WSBMainSeq3.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

