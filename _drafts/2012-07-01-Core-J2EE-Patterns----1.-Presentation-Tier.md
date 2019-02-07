---
title: "Core J2EE Patterns  - 1. Presentation Tier"
tags: [architecture, design-pattern, jee, oop]
date: 2012-07-01T18:22:00+09:00
---

PC 정리하다보니... 예전에 한참 공부할때 요약해둔 문서가 나왔다..

감회가 새록새록.. -0-;;

Core J2EE Design Patterns는 2000년도 초중반 Enterprise Application 개발에 정석처럼 활용되던 디자인 패턴들이고, 아직도 대다수의 Java 기반 웹 프레임워크 설계시에 많이 사용되고 있다.

단, IoC 패턴이 인기를 얻기 시작하고 AOP가 다방면에 활용되면서부터 J2EE의 일부 패턴들은 이제 거의 사용되지 않거나 변형되어 사용된다. 하지만 엔터프라이즈급 패턴들은 Object 단위의 GoF Patterns보다는 좀 더 고수준의 패턴이다보니 어플리케이션 설계나 객체 모델링에 관심이 있다면 아직도 읽어볼 가치가 충분하다.

각설하고~~!

Core J2EE Design Patterns Catalog는 [이전 포스팅](http://www.xenomity.com/Core-J2EE-Patterns) 참고.

#### **1. Intercepting Filter**

요청과 응답을 가로채고 변환, 조작하기 위한 디자인 패턴.

클라이언트의 요청과 응답에 대한 전후 처리를 FilterChain에 의해 sequential하게 처리할 수 있으며, 다양한 설정 방법들을 통해 Filter는 plug-in처럼 동작할 수 있다. 따라서, 요청을 처리하는 핸들러와 느슨한 연결이 가능해지고 Filter logic의 재사용성이 높아진다. 보통 Front Controller의 앞단에 위치하여, 검증, 인증, 권한 확인, 데이터 조작 등의 다양한 필터 역할을 수행한다. 또한, 각 Filter는 success 여부를 통해 다음 필터로 요청을 넘길지, 반려할지를 FilterChain에 의해 결정하게 할 수 있다.

Java EE Spec의 Standard Filter나 Spring Framework의 Interceptor가 이 패턴으로 설계되어 있다.

[##\_1C|cfile21.uf.2519EF4251D142343AAD91.gif|width="551" height="219" filename="IFMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile21.uf.272F3F4C51D142581F7A50.gif|width="590" height="481" filename="IFMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **2. Context Object**

프로토콜에 의존적인 객체를 독립적인 오브젝트로 재사용하는 디자인 패턴.

프로토콜이나 시스템에 의존적인 정보를 가진 객체를 별도의 독립적인 모델 오브젝트로 변환하여 각 계층에서 사용하도록 함으로서, 어플리케이션의 기술 의존도를 낮추고 재사용성 및 유지보수성, 테스트의 용이성을 높인다.

일반적으로 Context Object 타입으로 Map이나 POJO 형식의 Value Object를 사용한다.

(\* Spring MVC Controller의 가변적인 Arguments 중, 일부는 Context Object 패턴이 사용된다)

[##\_1C|cfile24.uf.27586F4951D14374020744.gif|width="426" height="209" filename="COMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile24.uf.24469D4951D1437410589C.gif|width="590" height="431" filename="COMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **3. Front Controller**

Presentation 계층 액세스에 대한 단일 진입 디자인 패턴.

다양한 요청에 대한 제어 로직의 중앙 집중화가 가능하고, 비니지스 로직에 대한 위임을 View로부터 분리시키기 쉬워진다. 또한 View에 대한 결정 및 Validation에 대한 프로세스도 중앙 집중화가 가능하므로, 보통 Application Controller, Intercepting Filter, Service To Worker, Dispatcher View 등의 패턴과 조합하여 함께 사용된다.

(\* Front Controller와 Application Controller가 함께 사용되는 경우, View에 대한 dispatching 및 validation process, business component로의 위임 등은 Application Controller에서 처리하게 되고, Front Controller는 AC를 핸들링하고 view resolving하는 제어 역할로 분리됨)

Java EE 환경에서는 일반적으로 Servlet이나 Filter로 개발되며, Spring Framework에서는 DispatcherServlet이 Front Controller 역할을 겸하고 있다.

(\* Model 2 시절에는 종종 JSP로 Front Controller를 디자인하는 케이스도 있었다..)

[##\_1C|cfile9.uf.013FED4851D144A32AB6AA.gif|width="590" height="209" filename="FCMainClass.gif" filemime="image/gif" style="text-align: center; "|\_##]

- Class Diagram -

[##\_1C|cfile8.uf.23488F4851D144A42559E9.gif|width="590" height="431" filename="FCMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **4. Application Controller**

다양한 요청에 대한 모듈화 및 View를 핸들링하기 위한 디자인 패턴.

하나의 요청과 응답에 대한 흐름을 관장하므로, View 핸들링 코드의 재사용이 수월해지고 제어 로직의 확장성이 증대된다. 일반적으로 하나의 요청에 따른 하나의 Application Controller를 매핑하여 설계(Command Handler Strategy)하므로, 유지보수성도 높아지고 단위 테스트도 간결해지며, 어플리케이션을 쉽게 확장할 수 있도록 한다.

(\* Spring @MVC의 @RequestMapping을 통해, 하나의 Application Controller 내에 operation(method) 단위로의 요청 처리가 가능)

(\* Spring MVC의 Controller에 해당)

Application Controller는 Front Controller에서 커맨드 핸들러 전략에 의해 결정되어지므로, 보통 단독으로는 사용하지 않는다. (Client에서 AC를 결정하는 코드가 생길 위험이 있기에...)

[##\_1C|cfile29.uf.034BFE4951D145B219D00A.gif|width="590" height="277" filename="ACMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile24.uf.0127894951D145B234C87F.gif|width="590" height="384" filename="ACMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **5. View Helper**

View를 생성하기 위한 프로세스 로직과 UI Code를 분리하기 위한 디자인 패턴.

View Helper는 Java EE Spec의 Custom Tags나 JavaBeans가 대표적인 예로, JSP와 같은 동적 페이지나 기타 템플릿 엔진 기반의 View에서, 프로세스 로직을 분리함으로서 명확한 역할의 분리와 UI 요소의 재사용이 가능하게 한다.

(\* 디자이너와의 협업 관계에서도 중요한 역할을 함)

Helper의 타겟은 프리젠테이션 모델 또는 Business Delegate가 될 수 있으며, View와 각 타겟 사이에서 adapter 역할을 수행하게 된다.

[##\_1C|cfile3.uf.0119C44151D146F50DCE61.gif|width="590" height="157" filename="VHMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile27.uf.2469F84151D146F63039A1.gif|width="590" height="366" filename="VHMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **6. Composite View**

모듈 단위별 View를 조합하여 전체 View를 완성하는 디자인 패턴.

Template layout을 정의할 때 대표적으로 사용된다. 여러 View에서 생길 수 있는 중복 코드를 크게 감소시킬 수 있으며, Decorator로 디자인된 레이아웃-템플릿 기반 설계보다는 좀 더 유연한 레이아웃 구성이 가능하다.

(\* 대표적인 Composite View로 디자인된 UI framework로는 Apache Tiles가 있음)

[##\_1C|cfile29.uf.24517C4051D147C032CFBE.gif|width="513" height="222" filename="CVMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile1.uf.2265874051D147C027F12B.gif|width="590" height="312" filename="CVMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **7. Dispatcher View**

요청에 따라 View를 핸들링하고, View 구성시에 Business Logic을 호출하는 상위 디자인 패턴.

클라이언트의 요청에 대하여, dispatching된 View에서 View Helper를 통해 비지니스 서비스로 접근하여 결과를 생성하는 방식으로, F/C, A/C, View Helper 패턴을 혼합하여 디자인하는 상위 패턴에 속한다.

(\* Service To Worker 패턴과의 차이는 Business Delegate 시점의 차이. Dispatcher View는 dispatching된 View에서 화면을 구성하는 시점에 동적으로 비지니스 티어에 접근하여 프로세싱된 데이터를 획득)

일반적인 UI framework에서 많이 볼 수 있는 thin-presentation 형식을 띈다.

[##\_1C|cfile22.uf.2122424D51D148E704A07E.gif|width="590" height="267" filename="DVMainClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile5.uf.261D0C4D51D148E808F66E.gif|width="590" height="269" filename="DVMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

#### **8. Service To Worker**

요청에 대하여 Business Logic을 호출하고 View로 dispatching하는 상위 디자인 패턴.

일반적인 웹 프레임워크의 presentation 계층에서 볼 수 있는 기초적인 디자인으로, MVC의 근간이 되는 패턴이다. Front Controller는 Business Service로 비지니스 처리를 위임하고, 응답받은 결과 모델을 View에서 표현할 수 있도록 View Helper를 이용한다. F/C와 A/C, View Helper 패턴을 혼합하여 디자인하는 상위 패턴이며, 요청에 대한 처리의 중앙 집중화 및 재사용성 증대, 모델과 View의 분리도가 높아진다.

(\* Dispatcher View 패턴과의 차이는 Business Delegate 시점의 차이. Service To Worker는 Controller에서 비지니스 서비스로 위임하여 얻은 결과를 request attribute로 View dispatching하여 데이터를 시각화하는 흐름)

(\* MVC 패턴의 뼈대 구조)

[##\_1C|cfile4.uf.2360AD4951D149E8206692.gif|width="590" height="263" filename="S2WMainHelperClass.gif" filemime="image/gif"|\_##]

- Class Diagram -

[##\_1C|cfile8.uf.255F334951D149E9211AD4.gif|width="590" height="268" filename="S2WMainSeq.gif" filemime="image/gif"|\_##]

- Sequence Diagram -

다음 포스팅은 비지니스 계층에 대한 요약~

이미지 출처: [http://www.corej2eepatterns.com](http://www.corej2eepatterns.com)

