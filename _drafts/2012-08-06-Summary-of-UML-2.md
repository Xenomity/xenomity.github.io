---
title: "Summary of UML 2"
tags: [architecture, modeling, oop, uml]
date: 2012-08-06T18:33:00+09:00
---

## 1. Overview
UML(Unified Modeling Language: [www.uml.org](http://www.uml.org/))은 시스템 소프트웨어를 설계하고 표현하는 것을 시각적으로 도와주기 위한 GML(Graphical Modeling Language)의 일종으로, 도메인의 추상적/피상적인 부분을 포함하여 교류 관계를 도식화하는 범용적인 '표준 객체 모델링 언어'이다. OMG(Object Management Group)에서 관리되는 개방 표준이며, 현재일자 기준 최신 명세 버전은 2.4.1이다.


## 2. UML Diagrams
| Category | Diagram | Description |
|----------|---------|-------------|
| Structure based | Class Diagrams | 클래스의 특성과 관계에 대한 묘사. |
| | Component Diagrams | 컴포넌트의 특성과 관계에 대한 묘사. |
| | Composite Structure Diagrams | 클래스의 내부 구조를 계층적 형식으로 묘사. |
| | Deployment Diagrams | 어플리케이션이 물리적 시스템에 배치된 구조와 동작하는 흐름을 묘사. |
| | Object Diagrams | 객체 인스턴스의 연관 관계를 묘사. |
| | Package Diagrams | 패키지 단위와 의존 관계를 묘사. |
| Behavior based | Activity Diagrams | 업무 절차에 대한 workflow를 묘사. |
| | Use Case Diagrams | 시스템과 사용자간의 교류를 단계적으로 묘사. |
| | State Machine Diagrams | 시스템의 행동을 기술. |
| | Sequence Diagrams | 한 가지 시나리오에 대한 순차적인 행동을 묘사. |
| | Communication Diagrams | 한 가지 시나리오에 대한 참여 요소들의 연결과 행동을 묘사. |
| | Interaction Overview Diagrams | Activity + Sequence Diagrams. |
| | Timing Diagrams | 다수의 객체들에 대한 타이밍 제약 조건을 묘사. |


### 2.1. Class Diagrams
객체의 타입과 객체간의 다양한 정적 관계에 대한 모습을 기술한다.

#### Property
- Property는 Attribute과 Association으로 표현할 수 있다.
- Attribute: syntax - __*{visibility} {name}:{type} {multiplicity}={default} {property-string}*__ (e.g. _**-name:String [1] = "Unnamed" {readOnly}**_)
- 정적(static) 속성의 경우 __밑줄(underline)__, final의 경우 _Italic_ 으로 표기하며, 상수의 경우(e.g. Java: static final) *__밑줄+이탤릭__* 형식으로 표기한다.
- visibility: +(public), -(private), ~(package), #(protected) - UML meta-model.
- Association: 클래스간의 연관 관계를 화살표로 표시. (unidirectional/bidirectional)


![class diagram 1](/assets/image/2012-08-06-class diagram 1.png) ![class diagram 2](/assets/image/2012-08-06-class diagram 2.png)

  - 그림 2.1.1. Attribute, Association으로 표현한 Class Diagram 예

#### Multiplicity
- Property에 할당될 수 있는 객체의 개수를 범위로 표현한다.
- **[1]**: only one.
- **[0..1]**: null or one.
- **[*]**: null or unlimited. 다중 값을 가지게 되는 경우 {ordered}, {unique}, {unordered}, {nonunique}, {bag} 등의 property-string을 지정하여 다중 값이 가지는 자료들의 특성을 명시적으로 기술할 수 있다.

#### Operation
- 클래스가 수행하는 행위를 나타내며, 이는 Class의 Method와 대응한다.
- syntax - __*{visibility} {name} ({parameters}) : {return type} {property-string}*__ (e.g. _**+clonePerson (name: String, age: Integer) : Person**_)

#### Note
- Diagram Comment.

![class diagram](/assets/image/2012-08-06-class diagram 3.png)

  - 그림 2.1.2. Note의 예

#### Dependency
- 두 객체 사이의 의존성을 명시한다.
- keywords: <<call>>, <<create>>, <<derive>>, <<instantiate>>, <<permit>>, <<realize>>, <<refine>>, <<substitute>>, <<trace>>, <<use>>, etc...

![class diagram](/assets/image/2012-08-06-class diagram 4.png)

  - 그림 2.1.3. Dependency 예

#### Interface
- <<interface>> keyword 또는 ![ball_and_socket_notation](/assets/image/2012-08-06-ball_and_socket_notation.png) 표기법을 통해 단순화된 표현 가능.

#### Abstract Class
- {abstract} label.
![class diagram](/assets/image/2012-08-06-class diagram 7.png)

  - 그림 2.1.4. Interface and Abstract Class 예

![class diagram](/assets/image/2012-08-06-class diagram 8.png)

  - 그림 2.1.5. Ball-and-Socket 표기법을 통한 Interface 간략화 예

#### Enumeration
- <<enumeration>> keyword.

#### Parameterized Class (Generic)
- Template Parameter and Template Parameter and __{type}<{parameterized class variable}::{parameterized class type}>__ bound element.

![class diagram](/assets/image/2012-08-06-class diagram 6.png)

  - 그림 2.1.6. Generic and Bound Element 예

### 2.2. Sequence Diagrams
한 가지 시나리오에 대한 행동을 순차적으로 묘사한다.
- Synchronous Message: ![sync_arrow](/assets/image/2012-08-06-sync_arrow.png)
- Asynchronous Message: ![async_arrow](/assets/image/2012-08-06-async_arrow.png)

![sequence diagram](/assets/image/2012-08-06-sequence diagram 1.png)

  - 그림 2.2.1. Sequence Diagram 예

#### Interaction Frame
- 반복 또는 조건문을 나타내기 위한 표기법. (e.g. alt, opt, par, loop, region, neg, ref, sd, etc...)
![sequence diagram](/assets/image/2012-08-06-sequence diagram 2.png)

  - 그림 2.2.2. Interaction Frame을 사용한 Sequence Diagram 예

### 2.3. Package Diagrams
클래스의 묶음 단위를 표현하는 다이어그램으로, UML에서는 '**namespace**'로 표현한다.

- syntax: _{depth_1}::{depth_2} ... {depth_#}_. (e.g. **_java/util/concurrent -> java::util::concurrent_** )

![package diagram](/assets/image/2012-08-06-package diagram 1.png)

  - 그림 2.3. Package Diagram 예

### 2.4. Deployment Diagrams
시스템의 물리 구조 내부에서 소프트웨어의 모듈이 하드웨어의 어느 부분에서 동작하고 있는지를 나타낸다. Deployment Diagram은 아래와 같은 요소로 구분된다.

- **Node** : 소프트웨어를 실행하는 장치(device).
- **Execution Environment** : OS, JVM 등과 같은 소프트웨어를 실행하는 launcher (or loader).
- **Artifact** : 소프트웨어의 물리적인 형태(e.g. exe, jar, dll, etc...).

Artifact는 문서 아이콘 또는 '<<artifact>>' 키워드로 표시할 수 있으며, node에 포함되는 개념이다. 그리고 Node와 Artifact는 태그(brace `{...}`)를 통해 부가 정보를 기술할 수 있다.
  
![deployment diagram](/assets/image/2012-08-06-deployment diagram 1.png)

  - 그림 2.4. Deployment Diagram 예

### 2.5. Use Case Diagrams
Use Case간의 연관 관계를 표현하기 위한 Diagram. Use Case 수준에 따라 상당히 복잡해질 수 있다.

![use case diagram](/assets/image/2012-08-06-use case diagram 1.png)

  - 그림 2.5. Use Case Diagram 예

### 2.6. State Machine Diagrams
시스템의 행동 상태를 표현한다.

- **Transition** : 한 상태에서 다른 상태로는 이동을 나타낸다.
- **Label** : 각 Transition이 만족하는 조건을 나타내며, `{trigger-signature} [guard] / {activity}`로 기술한다. (trigger-signature: 상태 변화 유발 이벤트, guard: transition 만족 조건, activity: transition 과정에서 수행되는 행동)

![state machine diagram](/assets/image/2012-08-06-state machine diagram 1.png)

  - 그림 2.6. State Machine Diagram 예

### 2.7. Activity Diagrams
순차 로직, 업무 절차, 워크 플로우를 기술하고 표현한다. Activity는 다음과 같은 feature가 존재한다.

- **Initial Node** : 시작점.
- **Fork** : 하나의 시작점에서 여러 개의 동시 흐름으로 분리되는 지점. (e.g. Parallel Programming....)
- **Join** : 동시 흐름이 다시 하나로 모이는 지점(e.g. 병렬 처리의 동기화 지점). Join에 대한 조건이 필요하거나 명시해야 할 필요가 있다면, `{joinSpec = ~boolean 식~ }` join specification(e.g. **_{joinSpec = 1000won \>= price}_** )을 Join notation에 기술한다.
- **Action** : 행동, 행위.
- **Decision** : 조건 분기.
- **Merge** : Decision에 의한 조건 행동의 끝 지점.
- **Final Node** : 종료 지점.
- **Partition** : 행동/행위 주체에 대한 구획.
- **Token** : 다이어그램이 복잡해질 경우, 토큰을 발행하여 흐름을 시각화하는데 도움이 된다.
- **Flow/Edge** : 두 액션의 연결을 표현.

![activity diagram](/assets/image/2012-08-06-activity diagram 2.png)

  - 그림 2.7.1. Activity Diagram과 Partitioning-Activity Diagram 예

![activity diagram](/assets/image/2012-08-06-activity diagram 5.png)

  - 그림 2.7.2. Flow/Edge를 표현하는 네 가지 방법

- **Signal** : time-signal에 따른 event 발생과 event handling.

![activity diagram](/assets/image/2012-08-06-activity diagram 4.png)

  - 그림 2.7.3. Signal-Activity Diagram 예

- **Pin** : Action에 input-parameters 정보가 필요할 경우 표시.
- **Transformation** : pre-action의 출력과 post-action의 입력이 일치하지 않는 경우, transformation expression 표기.

![activity diagram](/assets/image/2012-08-06-activity diagram 6.png)

  - 그림 2.7.4. Pin과 Transformation 예

- **Expansion Region** : 컬렉션의 모든 아이템에 대하여 Action이 한 번씩 일어나는 영역.
![activity diagram](/assets/image/2012-08-06-activity diagram 7.png)

  - 그림 2.7.5. Expansion Region과 줄여서 표현한 예

- **Flow Final** : 특정한 Flow를 종료하기 위한 표기 방법.
![activity diagram](/assets/image/2012-08-06-activity diagram 9.png)

  - 그림 2.7.6. Flow Final 예


### 2.8. Communication Diagrams
참여 요소들에 대한 연결 흐름을 강조하여 표현한다. 보통 순차적인 번호를 부여하여 메세지를 표기한다.

![communication diagram](/assets/image/2012-08-06-communication diagram 1.png)

  - 그림 2.8. Communication Diagram 예

### 2.9. Composite Structure Diagrams
복잡한 클래스의 내부 구조를 분해하여 표현한다.

![composite structure diagram](/assets/image/2012-08-06-composite structure diagram 1.png) ![composite structure diagram](/assets/image/2012-08-06-composite structure diagram 2.png)

  - 그림 2.9. Composite Structure Diagram 예


### 2.10. Component Diagrams
컴포넌트간의 연관 관계를 표현한다.
- 컴포넌트 표기법: ![component diagram](/assets/image/2012-08-06-component diagram 1.png) or '<<component>>' keyword.
- 인터페이스 연결 관계 표기법: ![component diagram](/assets/image/2012-08-06-ball_and_socket_notation.png).

![component diagram](/assets/image/2012-08-06-component diagram 3.png)

  - 그림 2.10. Component Diagram 예

### 2.11. Interaction Overview Diagrams
단순히 Activity 다이어그램과 Sequence 다이어그램을 접목시킨 것으로서, 표현이 어지러워 실제 많이 사용되지는 않는다.

![interaction overview diagram](/assets/image/2012-08-06-interaction overview diagram.png)

  - 그림 2.11. Interaction Overview Diagram 예

### 2.12. Timing Diagrams
여러 객체들에 대한 타이밍 제약 조건을 표현하며, 객체의 상태 변화 복잡도에 따라 선 또는 영역으로 나타낸다.

![timing diagram 1](/assets/image/2012-08-06-timing diagram 1.png) ![timing diagram 2](/assets/image/2012-08-06-timing diagram 2.png)

  - 그림 2.12. Timing Diagram 예

## 3. References
- UML: [www.uml.org](http://www.uml.org)
- SEWO UML Tutorial: [http://www.sewo.biz/](http://www.sewo.biz/)

