---
title: "Method Chaining Pattern"
date: 2012-09-07 08:26:28
tags: [java, "design pattern"]
---

# Method Chaining Pattern
**Method Chaining Pattern**(이하 MCP)은 함수형 언어에서 많이 볼 수 있는 디자인 패턴으로서 행위의 연속을 단순하게 표현할 수 있도록 돕는다. 각 행위를 담당하는 메서드 혹은 함수는 다시 자신의 레퍼런스를 응답함으로서 여러 줄에 걸쳐서 표현된 코드를 단순하고 직관적으로 표현할 수 있다. 다음은 자바에서 흔히 사용되는 `StringBuilder` 클래스의 예를 보여준다.

```
String text = new StringBuilder().append(1).append(2).append(3)
				.append(4).reverse().toString();
```

위의 예에서 보듯이 `StringBuilder` 클래스의 `append()`, `reverse()` 메서드는 자기 역할을 수행한 후 리턴 타입으로 자신의 레퍼런스를 다시 응답하고 있기에 차후 행위에 대한 호출이 '**Chaining**' 되고 있다. 만약 `StringBuilder` 클래스의 메서드가 MCP로 설계되어 있지 않았다면 아래와 같은 코드가 되었을 것이다.

```
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append(1);
stringBuilder.append(2);
stringBuilder.append(3);
stringBuilder.append(4);
stringBuilder.reverse();
		
String text = stringBuilder.toString();
```

MCP 디자인을 잘 적용하면 표현력이 높은 코드를 작성할 수 있도록 활력을 제공한다. 예를 들면 행위 기반이 아닌 구조적 데이터 관점 -*들여쓰기만 잘 한다면(^^;)*-과 비슷하게 코드를 작성할 수도 있다. 아래에 몇 가지 추가적인 예를 보여준다. MCP로 설계된 API를 사용하는 클라이언트 코드의 간결함과 직관성이 드러남을 볼 수 있다.

```
TreeAccessor.addNode("ID_1")
				.setName("Xenomity")
				.setSex("M")
				.getParentNode()
			.addNode("ID_2")
				.setName("홍길동")
				.setSex("M")
				.getParentNode()
				...
```

```
CriteriaQuery<Person> query = cb.createQuery(...)
				.select(...)
				.from(...)
				.where(...)
```

## Conclusion
하지만 MCP를 너무 남용하게 되면 행위를 나타내는 인터페이스가 모호해 질 수 있다는 단점이 있다. 일반적으로 정보의 획득과 변경에 대한 행위는 명확하게 분리(*e.g. getter/setter*)하여 디자인되어야 하고, 객체 내부의 데이터에 변경을 가하는 경우 명시적으로 응답 형식이 없는(*void*) 인터페이스가 직관성이 흐려지지 않는 설계이기 때문이다. 그러므로 유틸리티나 테스트 지원 도구 개발 등의 선별적 경우로 한정하여 이 패턴을 적용하는 것이 옳으며, 레이어 간의 의존성을 줄여야 하는 엔터프라이즈 아키텍쳐에서는 신중을 기해야 한다.

## Reference
[Martin Fowler: Method Chaining](http://martinfowler.com/dslCatalog/methodChaining.html)
