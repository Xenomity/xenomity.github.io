---
title: "Refactoring Catalog"
tags: [refactoring]
date: 2010-05-16T00:01:00+09:00
---

Martin Fowler의 리팩토링 카탈로그.

| Methods                           | Description |
|-----------------------------------|-------------|
| Extract Method                    | 특정 기능 또는 공통화 가능한 코드를 별도의 메서드로 추출. |
| Inline Method                     | 메서드 내부의 기능이 너무 단순하거나 공통적으로 사용되는 코드가 아니라면, 호출하는 메서드로 호출되는 메서드의 코드를 통합. |
| Inline Temp                       | 간단한 수식의 결과값을 저장하는 임시 변수를 제거. |
| Replace Temp with Query           | 수식의 결과값을 저장하는 임시 변수를 제거하고, 수식은 별도의 메서드로 추출. |
| Introduce Explaining Variable     | 직관적인 임시 변수명 사용. |
| Split Temporary Variable          | 루프 변수나 누적용 임시 변수가 아닌 경우, 각 대입마다 다른 이름의 임시 변수명 사용. |
| Remove Assignments to Parameters  | 메서드 내부에서 직접적인 파라메터 조작을 피하고 임시 변수로 할당. |
| Replace Method with Method Object | 지역변수 때문에 메서드 추출을 하기 힘든 긴 메서드의 경우, 메서드를 객체로 전환. |
| Substitute Algorithm              | 알고리즘을 더 분명한 내용으로 교체. |
| Move Method                       | 데이터 사용 주체가 더 높은 쪽으로 메서드 이동. |
| Move Field                        | 데이터 사용 빈도가 더 높은 쪽으로 필드 이동. |
| Extract Class                     | 두 개 이상의 기능이 하나의 클래스에 들어 있으면 별도의 클래스로 추출. |
| Inline Class                      | 클래스의 기능이 너무 작거나 불완전할 때, 다른 클래스와 통합. |
| Hide Delegate                     | 호출자와 제공자 사이의 의존성을 줄이기 위한 위임 패턴. |
| Remove Middle Man                 | 위임 메서드가 불가피하게 많이 생겨날 경우, 위임자 제거. |
| Introduce Foreign Method          | 제공자의 클래스에 메서드를 추가할 수 없다면, 호출자의 클래스에 제공자의 클래스 인스턴스를 인자로 받는 메서드를 추가하여 기능 확장. |
| Introduce Local Extension         | 제공자의 클래스에 메서드를 추가할 수 없다면, 재공자의 클래스를 상속하거나 Wrapper로 기능 확장. |
| Self Encapsulate Field            | 자식 클래스 혹은 자기 자신에게 필드는 은닉하고 읽기/쓰기 메서드(getter/setter)로 필드에 접근. |
| Replace Data Value with Object    | 데이터 항목을 객체로 전환. |
| Replace Array with Object         | 배열의 인자가 각지 다른 의미를 지니는 경우, Value Object로 전환. |
| Duplicate Observed Data           | Observer 패턴을 통한 레이어의 분리. |
| Change Unidirectional Association to Bidirectional  | 클래스의 단방향 참조를 양방향으로 전환. |
| Change Bidirectional Association to Unidirectional  | 클래스의 양방향 참조를 단방향으로 전환. |
| Replace Magic Number with Symbolic Constant         | 특수한 값을 가지는 숫자를 상수로 전환. |
| Encapsulate Field                 | 필드는 은닉하고 읽기/쓰기 메서드(getter/setter)로 필드에 접근. |
| Encapsulate Collection | 컬렉션을 은닉하기 위해 추가, 삭제 위임자를 제공하고 수정 불가한 컬렉션으로 반환. |
| Replace Record with Data Class    | 데이터 항목을 Value Object로 전환. |
| Replace Type Code with Class      | 숫자형 분류 코드를 클래스 또는 열거자로 변경. |
| Replace Type Code with Subclasses | 분류 코드별 하위 클래스 전환. |
| Replace Type Code with State/Strategy               | 분류 코드를 State or Strategy 패턴으로 전환. |
| Replace Subclass with Field       | 여러 하위클래스가 상수를 반환하는 메서드만 다르다면, 각 하위클래스의 메서드를 상위클래스의 필드로 전환. |
| Decompose Conditional             |  복잡한 조건문의 경우, 각 절을 메서드로 추출. (if, if else, else) |
| Consolidate Conditional Expression | 중복되는 조건문은 하나로 통합. |
| Consolidate Duplicate Conditional Fragments         | 조건문의 공통 코드 추출. |
| Remove Control Flag               | 복잡한 루프문의 제어 플래그를 break, continue, return문으로 변경. |
| Replace Nested Conditional with Guard Clauses       | 메서드의 조건문이 정상적인 실행 경로를 파악하기 힘들 땐 감시 절을 사용. |
| Replace Conditional with Polymorphism               | 조건문의 각 절을 하위클래스의 오버라이딩 메서드로 정의. |
| Introduce Null Object             | null 검사가 자주 필요하다면, 해당 객체를 Null 객체로 전환. |
| Introduce Assertion               | Assertion을 통한 전제 조건을 명시적으로 검증. |
| Rename Method                     | 직관적인 메서드명 사용. |
| Add Parameter                     | 메서드가 필요로 하는 외부 정보를 파라메터로 전달. |
| Remove Parameter                  | 사용하지 않는 파라메터 제거. |
| Separate Query from Modifier      | 값 수정/획득 기능이 한 메서드에서 제공된다면, 명시적인 getter/setter 메서드로 분리. |
| Parameterize Method               | 기능이 비슷한 메서드를 서로 다른 값을 하나의 파라메터로 전달받는 메서드로 전환. |
| Replace Parameter with Explicit Methods             | 파라메터별 다른 기능을 수행하는 하나의 메서드는 기능별 메서드로 분리. |
| Preserve Whole Object             | 특정 객체의 여러 값을 파라메터로 전달할 경우, 특정 객체를 직접 전달. |
| Replace Parameter with Method     | 메서드가 내부에서 호출 가능한 다른 메서드가 있을 때, 파라메터로 값을 전달받는 대신 특정 메서드를 직접 호출. |
| Introduce Parameter Object        | 여러 개의 파라메터 집합을 객체로 전환하여 파라메터 길이를 줄임. |
| Remove Setting Method             | 읽기 전용 필드의 경우, setter 메서드를 제거. |
| Hide Method                       | 다른 클래스에서 사용되지 않는 메서드를 은닉. |
| Replace Constructor with Factory Method             | 단순 객체 생성은 factory method 패턴으로 전환. |
| Encapsulate Downcast              | 다운캐스트는 호출자의 코드가 아닌, 제공자 메서드 내부에서 실시. |
| Replace Error Code with Exception | 에러 코드 반환 방식의 코드는 예외 발생 코드로 전환. |
| Replace Exception with Test       | 테스트 가능한 코드는 예외 발생 대신 사전 검사 코드로 전환. |
| Pull Up Field                     | 공통화 가능한 필드는 상위클래스로 이동. |
| Pull Up Method                    | 공통화 가능한 메서드는 상위클래스로 이동. |
| ull Up Constructor Body           | 비슷한 내용의 생성자를 상위클래스에 작성하고, 하위클래스에서 상위클래스의 생성자 호출. |
| Push Down Method                  | 상위클래스의 일부 기능만 하위클래스에서 사용되는 경우, 메서드 하향. |
| Push Down Field                   | 상위클래스의 일부 필드만 하위클래스에서 사용되는 경우, 필드 하향. |
| Extract Subclass                  | 일부 인스턴스에만 사용되는 기능의 클래스의 경우, 그 기능 부분을 전담하는 하위클래스 생성. |
| Extract Superclass                | 비슷한 기능의 여러 클래스가 존재하는 경우, 상위클래스로 공통 기능을 추출. |
| Extract Interface                 | 재사용 가능한 특정 기능에 대한 인터페이스를 추출. |
| Collapse Hierarchy                | 상위클래스와 하위클래스가 거의 비슷하다면 클래스 병합.
| Form Template Method              | Template Method 패턴 사용. |
| Replace Inheritance with Delegation | 하위클래스가 상위클래스의 일부 기능만 사용하거나 적절히 래핑된 기능을 제공해야 하는 경우, 상속을 위임으로 전환. |
| Replace Delegation with Inheritance | 특정 클래스가 간단한 위임 메서드만으로 도배되는 경우, 위임을 상속으로 전환. |
| Separate Domain from Presentation | 도메인 로직을 표현과 분리. (e.g. MVC, MVVM, MVP, etc) |
| Extract Hierarchy                 | 특정 클래스에 기능 또는 조건문이 많을 때, 각 조건에 해당하는 하위클래스를 통한 계층 구조 설계. |

