---
title: "Core J2EE Patterns  - 2. Business Tier"
tags: [architecture, design-pattern, jee, oop]
date: 2012-07-01T18:51:00+09:00
---

#### **1. Business Delegate**

비지니스 서비스를 호출하기 위한 과정을 은닉하고 위임하는 디자인 패턴.

여러 다른 환경이나 특정 기술에 의존하고 있는 비지니스 계층의 서비스를 호출하기 위해서, Business Delegate는 클라이언트로부터 복잡한 원격 호출 과정이나 특정 기술에 의존하는 코드 등의 호출 전처리 과정을 감추고, 단순한 인터페이스만 노출시킨다. 그리고 특정 인터페이스의 exception을 사용자 정의 business exception으로 wrapping하여 처리할 수도 있으므로, 클라이언트의 코드는 재사용성과 가용성이 증대된다.

실제 비지니스 서비스의 proxy 역할을 하며, 서비스 lookup을 위해 Service Locator를 사용한다.

[##\_1C|cfile23.uf.01076D4851D14B531C6378.gif|width="566" height="245" filename="BDMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile4.uf.2476774851D14B54294F1A.gif|width="590" height="525" filename="BDMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **2. Service Locator**

클라이언트가 투명하게 비지니스 계층의 컴포넌트나 서비스를 호출하게 하는 디자인 패턴.

원래는 JNDI lookup하는 과정을 캡슐화하고, 획득한 리소스에 대한 캐싱 위주로 EJB에서 많이 사용되던 패턴이지만, 실제로는 비지니스 서버가 어떤 기술을 사용하던지 상관없이 다양한 리소스에 대한 추상화된 인터페이스를 제공함이 그 목표이다. 보통 Business Delegate를 통해 사용된다.

(\* Business Delegate는 비지니스 처리를 위임하기 위해 Service Locator를 사용한다. 역할은 비슷해 보일 수 있지만, Service Locator는 접근 자원에 대한 connection, 캐싱 및 은닉화를, Business Delegate는 호출할 비지니스 서비스에 대한 캐싱 및 세부 구현을 숨기므로, 기술 의존 요소와 비지니스 위임에 대한 역할 구분이 명확하다)

(\* 다양한 Web Service에 대한 접근자로 Service Locator 패턴을 많이 사용함)

[##\_1C|cfile6.uf.0325EE5051D14C6518EBC1.gif|width="490" height="374" filename="SLMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile26.uf.031F945051D14C661D8877.gif|width="590" height="441" filename="SLMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **3. Session Facade**

Business Component 집합에 대한 상위 인터페이스를 클라이언트에게 노출하는 디자인 패턴.

GoF의 Facade 패턴과 동일한 디자인이지만, EJB 환경에 특화된 패턴이다. 딱히 EJB 환경이 아니더라도 Facade(POJO)의 operation(method) 단위 트랜잭션 적용 및 비지니스 플로우를 캡슐화하는 등 다양한 설계상의 이점을 제공한다.

(\* Spring Framework의 비지니스 계층에서 Service Facade + Business Object 전략이 많이 사용됨. 관련 내용은 이전 포스팅 참고: [http://www.xenomity.com/105](http://www.xenomity.com/105))

[##\_1C|cfile10.uf.2319704A51D14CE31414AC.gif|width="590" height="171" filename="SFMainClass.gif" filemime="image/gif" style="text-align: center; "|\_##]

- Class Diagram -

[##\_1C|cfile29.uf.2436EF4A51D14CE3015EA2.gif|width="590" height="448" filename="SFMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **4. Application Service**

여러 비지니스 계층의 컴포넌트들이나 서비스들을 중압 집중화하는 디자인 패턴.

보통 Facade와 Business Object 사이에서 중간자적 역할을 담당하며, Facade에서 추상화된 비지니스 로직에 대한 역할을 하게됨으로서 Facade간의 중복 코드 방지에는 좋지만, Business Object로 처리를 위임하는 단계가 추가적으로 정의된다는 단점도 있다.

(\* 잘 설계된 Service Facade와 Business Object라면, Application Service는 추가적인 레이어로 참여하므로 장점보다 단점이 더 클 수도 있음.) 

[##\_1C|cfile7.uf.2207AF3F51D14D9014FBA7.gif|width="590" height="255" filename="ASMainClass.gif" filemime="image/gif" style="text-align: center; "|\_##]

- Class Diagram -

[##\_1C|cfile23.uf.24166F3F51D14D900BBA9B.gif|width="590" height="402" filename="ASMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **5. Business Object**

객체 모델을 통한 비지니스 로직과 데이터를 분리하는 디자인 패턴.

이름 그대로 비지니스 로직을 담고있는 객체이며, 객체 모델로는 Transfer Object를 사용한다. 비지니스 로직의 재사용성을 높히기 위해 operation은 최소 단위로 쪼개고, Facade 패턴과 함께 사용되는 디자인이 일반적이다.

Business Object는 환경에 따라 POJO, Session Bean, Entity Bean 등으로 구현될 수 있다.

[##\_1C|cfile26.uf.2711574051D14E5A276745.gif|width="590" height="197" filename="BOMainClass.gif" filemime="image/gif" style="text-align: center; "|\_##]

- Class Diagram -

[##\_1C|cfile9.uf.0206AB4051D14E5B307CC3.gif|width="590" height="691" filename="BOMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **6. Composite Entity**

개념적인 도메인 모델(Business Object)을 Entity Bean으로 구현하는 EJB 디자인 패턴.

비지니스 처리에 필요한 데이터셋(여러 Entity Bean들)을 하나의 Entity Bean에 포함하므로서, 비지니스 요청은 Composite Entity에 대한 단일 접근 처리가 가능하다. 따라서 비지니스 계층의 복잡한 인터페이스는 단순화되며, Entity Bean의 감소로 인해 원격 호출 횟수를 크게 줄일 수 있다.

(\* 초창기 EJB의 성능 이슈를 어느정도 해결해 줄 수 있었던 패턴)

[##\_1C|cfile8.uf.2669AD3F51D14F0B2DA8E0.gif|width="590" height="224" filename="CEMainClass.gif" filemime="image/gif" style="text-align: center; "|\_##]

- Class Diagram -

[##\_1C|cfile25.uf.245EA83F51D14F0C37833B.gif|width="590" height="350" filename="CEMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **7. Transfer Object**

계층간 데이터 전달을 위한 디자인 패턴.

Transfer Object는 계층간의 데이터 전달을 캡슐화하고, 좀 더 큰 단위의 객체로 접근하게 함으로서, 비지니스 계층의 API를 단순화시키고 네트워크 트래픽을 줄일 수 있다. (한번에 많은 데이터를 wrapping하여 전달할 수 있으므로...)

getter/setter로 구성되는 data wrapper 모델.

(\* 참고로 Data Transfer Object(DTO), Value Object(VO), Transfer Object(TO)에 대한 재미있는 비교글이 있다. [http://www.coderanch.com/t/154686/java-Architect-SCEA/certification/Object-VO-Transfer-Object-Data](http://www.coderanch.com/t/154686/java-Architect-SCEA/certification/Object-VO-Transfer-Object-Data))

(\* Custom hash-map(key별로 type-safe)도 Transfer Object의 하나의 대안일 수 있지만, 성능적 우위나 오퍼레이션의 직관성은 TO가 더 낫다) 

[##\_1C|cfile3.uf.251B8F4951D14F93117304.gif|width="491" height="245" filename="TOMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile21.uf.0102F74951D14F931F7036.gif|width="517" height="571" filename="TOMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **8. Transfer Object Assembler**

복합 Transfer Object를 조합하는 디자인 패턴.

TOA는 다양한 비지니스 서비스를 호출하고 결과 모델(Transfer Object)들을 하나의 Transfer Object로 wrapping하여 클라이언트로 응답한다. 보통 분산 환경에서 네트워크 트래픽을 줄이기 위한 목적으로 사용되며, 이때는 Service Locator와 같은 역할을 하게 된다.

(\* 단일 JVM 환경이라면, 구지 server-side에서 데이터를 조합하여 응답하는 TOA 디자인보다는 클라이언트 입장에서 필요한 비지니스 서비스를 순차 호출하고 결과셋을 조합하여 사용하는 편이 더 나아보인다... 비지니스 서비스는 최소 단위를 유지할 수 있어야 하며, API는 유연하고 투명성이 좋아야 함) 

[##\_1C|cfile25.uf.2248594F51D1505926EDD9.gif|width="586" height="367" filename="TOAClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile6.uf.25595C4F51D150591BA681.gif|width="590" height="311" filename="TOASeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **9. Value List Handler**

클라이언트가 이터레이션 방식으로 원하는 부분 데이터만 획득하기 위한 디자인 패턴.

클라이언트는 이터레이션 방식으로 데이터를 획득할 수 있으며, 필요 데이터만 응답받게 되므로 네트워크 트래픽을 감소시킬 수 있다. 보통, 캐싱 전략을 함께 사용한다.

[##\_1C|cfile1.uf.271DAE4C51D150D41FA1FF.gif|width="590" height="289" filename="VLHMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile7.uf.2109E54C51D150D4290D3D.gif|width="590" height="499" filename="VLHMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -
