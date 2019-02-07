---
title: "Summary of UML 2"
tags: [architecture, modeling, oop, uml]
date: 2012-08-06T18:33:00+09:00
---

#### **1. Overview**

UML(Unified Modeling Language: [www.uml.org](http://www.uml.org/))은 시스템 소프트웨어를 설계하고 표현하는 것을 시각적으로 도와주기 위한 GML(Graphical Modeling Language)의 일종으로, 도메인의 추상적/피상적인 부분을 포함하여 교류 관계를 도식화하는 범용적인 '표준 객체 모델링 언어'이다. OMG(Object Management Group)에서 관리되는 개방 표준이며, 현재일자 기준 최신 명세 버전은 2.4.1이다.

#### **2. UML Diagrams**

| 

 Category

 | 

 Diagram

 | 

 Description

 |
| 

 Structure based

 | 

 Class Diagrams

 | 

 클래스의 특성과 관계에 대한 묘사.

 |
| 

 Component Diagrams

 | 

 컴포넌트의 특성과 관계에 대한 묘사.

 |
| 

 Composite Structure Diagrams

 | 

 클래스의 내부 구조를 계층적 형식으로 묘사.

 |
| 

 Deployment Diagrams

 | 

 어플리케이션이 물리적 시스템에 배치된 구조와 동작하는 흐름을 묘사.

 |
| 

 Object Diagrams

 | 

 객체 인스턴스의 연관 관계를 묘사.

 |
| 

 Package Diagrams

 | 

 패키지 단위와 의존 관계를 묘사.

 |
| 

 Behavior based

 | 

 Activity Diagrams

 | 

 업무 절차에 대한 workflow를 묘사.

 |
| 

 Use Case Diagrams

 | 

 시스템과 사용자간의 교류를 단계적으로 묘사.

 |
| 

 State Machine Diagrams

 | 

 시스템의 행동을 기술.

 |
| 

 Sequence Diagrams

 | 

 한 가지 시나리오에 대한 순차적인 행동을 묘사.

 |
|  Communication Diagrams | 

 한 가지 시나리오에 대한 참여 요소들의 연결과 행동을 묘사.

 |
|  Interaction Overview Diagrams | 

 Activity + Sequence Diagrams.

 |
|  Timing Diagrams | 

 다수의 객체들에 대한 타이밍 제약 조건을 묘사.

 |

##### **2.1. Class Diagrams**

객체의 타입과 객체간의 다양한 정적 관계에 대한 모습을 기술한다.

- **Property** : Property는 Attribute과 Association으로 표현할 수 있다.

 i) Attribute: syntax - <u><i>{visibility} {name}:{type} {multiplicity}={default} {property-string}</i></u> (e.g. _**-name:String [1] = "Unnamed" {readOnly}**_)

 i-1) 정적(static) 속성의 경우 <u>밑줄(underline)</u>, final의 경우 _Italic_ 으로 표기하며, 상수의 경우(e.g. Java: static final) _밑줄+이탤릭_ 형식으로 표기한다.

 i-2) visibility: +(public), -(private), ~(package), #(protected) - UML meta-model.

 ii) Association: 클래스간의 연관 관계를 화살표로 표시. (unidirectional/bidirectional)

[##\_1C|cfile5.uf.253A7737540D43B81B08F0.png|width="184" height="100" filename="class diagram 1.png" filemime="image/jpeg" style="width: 184px; height: 100px;"|\_##] [##\_1C|cfile6.uf.2317B337540D43B93BDFA4.png|width="293" height="100" filename="class diagram 2.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 293px; height: 100px; background-color: transparent;"|\_##]

- 그림 2.1.1. Attribute, Association으로 표현한 Class Diagram 예 -

- **Multiplicity** : Property에 할당될 수 있는 객체의 개수를 범위로 표현한다.

 i) **<u>[1]</u>**: only one.

 ii) **<u>[0..1]</u>**: null or one.

 iii) **<u>[*]</u>**: null or unlimited. 다중 값을 가지게 되는 경우 {ordered}, {unique}, {unordered}, {nonunique}, {bag} 등의 property-string을 지정하여 다중 값이 가지는 자료들의 특성을 명시적으로 기술할 수 있다.

- **Operation** : 클래스가 수행하는 행위를 나타내며, 이는 Class의 Method와 대응한다.

 i) syntax - <u><i>{visibility} {name} ({parameters}) : {return type} {property-string}</i></u> (e.g. _**+clonePerson (name: String, age: Integer) : Person**_)

- **Note** : Diagram Comment.

[##\_1C|cfile3.uf.2705BA4E540D4BC70D617B.png|width="296" height="80" filename="class diagram 3.png" filemime="image/jpeg" style="width: 296px; height: 80px;"|\_##]

- 그림 2.1.2. Note의 예 -

- **Dependency** : 두 객체 사이의 의존성을 명시한다.

 i) keywords: \<\<call\>\>, \<\<create\>\>, \<\<derive\>\>, \<\<instantiate\>\>, \<\<permit\>\>, \<\<realize\>\>, \<\<refine\>\>, \<\<substitute\>\>, \<\<trace\>\>, \<\<use\>\>, etc...

[##\_1C|cfile24.uf.2143553F540D4E8532CA40.png|width="303" height="100" filename="class diagram 4.png" filemime="image/jpeg" style="width: 303px; height: 100px;"|\_##]

- 그림 2.1.3. Dependency 예 -

- **Interface** : \<\<interface\>\> keyword 또는 [##\_1C|cfile24.uf.2641D241540CA507115294.png|width="13" height="12" filename="ball\_and\_socket\_notation.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 13px; height: 12px; background-color: transparent;"|\_##](ball-and-socket) 표기법을 통해 단순화된 표현 가능.

- **Abstract Class** : {abstract} label.

[##\_1C|cfile2.uf.25660F4A540D5F6C03A6F4.png|width="332" height="300" filename="class diagram 7.png" filemime="image/jpeg" style="width: 332px; height: 300px;"|\_##]

- 그림 2.1.4. Interface and Abstract Class 예 -

[##\_1C|cfile24.uf.21351D4C540D5FEB2E9177.png|width="257" height="70" filename="class diagram 8.png" filemime="image/jpeg" style="width: 257px; height: 70px;"|\_##]

- 그림 2.1.5. Ball-and-Socket 표기법을 통한 Interface 간략화 예 -

- **Enumeration** : \<\<enumeration\>\> keyword.

- **Parameterized Class (Generic)**: Template Parameter and <u style="font-style: italic;">{type}&lt;{parameterized class variable}::{parameterized class type}&gt;</u> bound element.

[##\_1C|cfile21.uf.22168738540D5B3C08C2B0.png|width="295" height="200" filename="class diagram 6.png" filemime="image/jpeg" style="width: 295px; height: 200px;"|\_##]

- 그림 2.1.6. Generic and Bound Element 예 -

##### **2.2. Sequence Diagrams**

한 가지 시나리오에 대한 행동을 순차적으로 묘사한다.

- Synchronous Message: [##\_1C|cfile6.uf.27577B3D540D528C38B663.png|width="16" height="12" filename="sync\_arrow.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 16px; height: 12px; background-color: transparent;"|\_##]

- Asynchronous Message: [##\_1C|cfile6.uf.2764633E540D52B31C7783.png|width="15" height="12" filename="async\_arrow.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 15px; height: 12px; background-color: transparent;"|\_##]

[##\_1C|cfile23.uf.2668E942540D4F7B2F543A.png|width="364" height="300" filename="sequence diagram 1.png" filemime="image/jpeg" style="width: 364px; height: 300px;"|\_##]

- 그림 2.2.1. Sequence Diagram 예 -

- **Interaction Frame** : 반복 또는 조건문을 나타내기 위한 표기법. (e.g. alt, opt, par, loop, region, neg, ref, sd, etc...)

[##\_1C|cfile25.uf.263BD434540D507F0B7BD9.png|width="362" height="300" filename="sequence diagram 2.png" filemime="image/jpeg" style="width: 362px; height: 300px;"|\_##]

- 그림 2.2.2. Interaction Frame을 사용한 Sequence Diagram 예 -

##### **2.3. Package Diagrams**

클래스의 묶음 단위를 표현하는 다이어그램으로, UML에서는 ' **namespace**'로 표현한다.

- syntax: _<u>{depth_1}::{depth_2} ... {depth_#}</u>_. (e.g. **_java/util/concurrent -\> java::util::concurrent_** )

[##\_1C|cfile7.uf.23325C44540C9AF00A4994.png|width="226" height="160" filename="package diagram 1.png" filemime="image/jpeg" style="width: 226px; height: 160px;"|\_##] [##\_1C|cfile30.uf.21328644540C9AF10AC18F.png|width="313" height="160" filename="package diagram 2.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 313px; height: 160px; background-color: transparent;"|\_##]

- 그림 2.3. Package Diagram 예 -

##### **2.4. Deployment Diagrams**

시스템의 물리 구조 내부에서 소프트웨어의 모듈이 하드웨어의 어느 부분에서 동작하고 있는지를 나타낸다. Deployment Diagram은 아래와 같은 요소로 구분된다.

- **Node** : 소프트웨어를 실행하는 장치(device).

- **Execution Environment** : OS, JVM 등과 같은 소프트웨어를 실행하는 launcher (or loader).

- **Artifact** : 소프트웨어의 물리적인 형태(e.g. exe, jar, dll, etc...).

Artifact는 문서 아이콘 또는 '\<\<artifact\>\>' 키워드로 표시할 수 있으며, node에 포함되는 개념이다. 그리고 Node와 Artifact는 태그(brace '{...}')를 통해 부가 정보를 기술할 수 있다.

[##\_1C|cfile21.uf.22328D44540CAA01226ABB.png|width="347" height="300" filename="deployment diagram 1.png" filemime="image/jpeg" style="width: 347px; height: 300px;"|\_##]

- 그림 2.4. Deployment Diagram 예 -

##### **2.5. Use Case Diagrams**

Use Case간의 연관 관계를 표현하기 위한 Diagram. Use Case 수준에 따라 상당히 복잡해질 수 있다.

[##\_1C|cfile3.uf.246B244B540D60A1238B6B.png|width="311" height="200" filename="use case diagram 1.png" filemime="image/jpeg" style="width: 311px; height: 200px;"|\_##]

- 그림 2.5. Use Case Diagram 예 -

##### **2.6. State Machine Diagrams**

시스템의 행동 상태를 표현한다.

- **Transition** : 한 상태에서 다른 상태로는 이동을 나타낸다.

- **Label** : 각 Transition이 만족하는 조건을 나타내며, <u><i>{trigger-signature} [guard] / {activity}</i></u>로 기술한다. (trigger-signature: 상태 변화 유발 이벤트, guard: transition 만족 조건, activity: transition 과정에서 수행되는 행동)

[##\_1C|cfile26.uf.232E4737540D6DA62659C9.png|width="323" height="200" filename="state machine diagram 1.png" filemime="image/jpeg" style="width: 323px; height: 200px;"|\_##]

- 그림 2.6. State Machine Diagram 예 -

##### **2.7. Activity Diagrams**

순차 로직, 업무 절차, 워크 플로우를 기술하고 표현한다. Activity는 다음과 같은 feature가 존재한다.

- **Initial Node** : 시작점.

- **Fork** : 하나의 시작점에서 여러 개의 동시 흐름으로 분리되는 지점. (e.g. Parallel Programming....)

- **Join** : 동시 흐름이 다시 하나로 모이는 지점(e.g. 병렬 처리의 동기화 지점). Join에 대한 조건이 필요하거나 명시해야 할 필요가 있다면, '<u><i>{joinSpec = ~boolean 식~ }</i></u>' join specification(e.g. **_{joinSpec = 1000won \>= price}_** )을 Join notation에 기술한다.

- **Action** : 행동, 행위.

- **Decision** : 조건 분기.

- **Merge** : Decision에 의한 조건 행동의 끝 지점.

- **Final Node** : 종료 지점.

- **Partition** : 행동/행위 주체에 대한 구획.

- **Token** : 다이어그램이 복잡해질 경우, 토큰을 발행하여 흐름을 시각화하는데 도움이 된다.

- **Flow/Edge** : 두 액션의 연결을 표현.

[##\_1C|cfile25.uf.26595043540DE37B1E28D0.png|width="323" height="500" filename="activity diagram 1.png" filemime="image/jpeg" style="width: 323px; height: 500px;"|\_##] [##\_1C|cfile24.uf.26360850540DE5EA046276.png|width="334" height="400" filename="activity diagram 2.png" filemime="image/jpeg" style="text-align: left; font-size: 9pt; line-height: 1.5; width: 334px; height: 400px; background-color: transparent;"|\_##]

- 그림 2.7.1. Activity Diagram과 Partitioning-Activity Diagram 예 -

[##\_1C|cfile1.uf.2320A13A540DEE050E6BCB.png|width="288" height="200" filename="activity diagram 5.png" filemime="image/jpeg" style="width: 288px; height: 200px;"|\_##]

- 그림 2.7.2. Flow/Edge를 표현하는 네 가지 방법 -

- **Signal** : time-signal에 따른 event 발생과 event handling.

[##\_1C|cfile26.uf.26304F4E540DEBCA216005.png|width="301" height="140" filename="activity diagram 3.png" filemime="image/jpeg" style="width: 301px; height: 140px;"|\_##] [##\_1C|cfile28.uf.272E2A4E540DEBCB22C0C6.png|width="326" height="140" filename="activity diagram 4.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 326px; height: 140px; background-color: transparent;"|\_##]

- 그림 2.7.3. Signal-Activity Diagram 예 -

- **Pin** : Action에 input-parameters 정보가 필요할 경우 표시.

- **Transformation** : pre-action의 출력과 post-action의 입력이 일치하지 않는 경우, transformation expression 표기.

[##\_1C|cfile4.uf.274D8E39540DF042145F17.png|width="321" height="160" filename="activity diagram 6.png" filemime="image/jpeg" style="width: 321px; height: 160px;"|\_##]

- 그림 2.7.4. Pin과 Transformation 예 -

- **Expansion Region** : 컬렉션의 모든 아이템에 대하여 Action이 한 번씩 일어나는 영역.

[##\_1C|cfile6.uf.223DDD42540DF2011C8EE4.png|width="287" height="140" filename="activity diagram 7.png" filemime="image/jpeg" style="width: 287px; height: 140px;"|\_##] [##\_1C|cfile28.uf.253C6842540DF2021DCBEE.png|width="288" height="40" filename="activity diagram 8.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 288px; height: 40px; background-color: transparent;"|\_##]

- 그림 2.7.5. Expansion Region과 줄여서 표현한 예 -

- **Flow Final** : 특정한 Flow를 종료하기 위한 표기 방법.

[##\_1C|cfile28.uf.222B133F540DF37F13C6E3.png|width="360" height="300" filename="activity diagram 9.png" filemime="image/jpeg" style="width: 360px; height: 300px;"|\_##]

- 그림 2.7.6. Flow Final 예 -

##### **2.8. Communication Diagrams**

참여 요소들에 대한 연결 흐름을 강조하여 표현한다. 보통 순차적인 번호를 부여하여 메세지를 표기한다.

[##\_1C|cfile22.uf.236C534B540D62D828232A.png|width="411" height="200" filename="communication diagram 1.png" filemime="image/jpeg" style="width: 411px; height: 200px;"|\_##]

- 그림 2.8. Communication Diagram 예 -

##### **2.9. Composite Structure Diagrams**

복잡한 클래스의 내부 구조를 분해하여 표현한다.

[##\_1C|cfile25.uf.225CB54C540D68D51B9775.png|width="281" height="300" filename="composite structure diagram 1.png" filemime="image/jpeg" style="width: 281px; height: 300px;"|\_##][##\_1C|cfile1.uf.2271C54C540D68D6058EA7.png|width="218" height="140" filename="composite structure diagram 2.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 218px; height: 140px; background-color: transparent;"|\_##]

- 그림 2.9. Composite Structure Diagram 예 -

##### **2.10. Component Diagrams**

컴포넌트간의 연관 관계를 표현한다.

- 컴포넌트 표기법: [##\_1C|cfile1.uf.2276BE42540CA35A1E5D1A.png|width="40" height="20" filename="component diagram 1.png" filemime="image/jpeg" style="text-align: center; font-size: 9pt; line-height: 1.5; width: 40px; height: 20px; background-color: transparent;"|\_##] or '\<\<component\>\>' keyword.

- 인터페이스 연결 관계 표기법:[##\_1C|cfile24.uf.2641D241540CA507115294.png|width="13" height="12" filename="ball\_and\_socket\_notation.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 13px; height: 12px; background-color: transparent;"|\_##](ball-and-socket).

[##\_1C|cfile22.uf.237DBD39540CA5471C0DE4.png|width="350" height="200" filename="component diagram 3.png" filemime="image/jpeg" style="width: 350px; height: 200px;"|\_##]

- 그림 2.10. Component Diagram 예 -

##### **2.11. Interaction Overview Diagrams**

단순히 Activity 다이어그램과 Sequence 다이어그램을 접목시킨 것으로서, 표현이 어지러워 실제 많이 사용되지는 않는다.

[##\_1C|cfile10.uf.24601F3F540C9E400F7F06.png|width="340" height="400" filename="interaction overview diagram.png" filemime="image/jpeg" style="width: 340px; height: 400px;"|\_##]

- 그림 2.11. Interaction Overview Diagram 예 -

##### **2.12. Timing Diagrams**

여러 객체들에 대한 타이밍 제약 조건을 표현하며, 객체의 상태 변화 복잡도에 따라 선 또는 영역으로 나타낸다.

[##\_1C|cfile29.uf.257C8C40540C9FFF0E2388.png|width="343" height="200" filename="timing diagram 1.png" filemime="image/jpeg" style="width: 343px; height: 200px;"|\_##] [##\_1C|cfile29.uf.247CC240540CA0000E2FBB.png|width="262" height="200" filename="timing diagram 2.png" filemime="image/jpeg" style="font-size: 9pt; line-height: 1.5; width: 262px; height: 200px; background-color: transparent;"|\_##]

- 그림 2.12. Timing Diagram 예 -

#### **3. References**

UML: [www.uml.org](http://www.uml.org)

SEWO UML Tutorial: [http://www.sewo.biz/](http://www.sewo.biz/)

