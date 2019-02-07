---
title: "Spring Business Tier Design Pattern"
tags: [architecture, design-pattern, jee, oop, spring]
date: 2011-11-29T12:11:35+09:00
---

회사에서 문서질만 하다 Spring @Service를 이용한 Business Tier에서 설계할 수 있는 Design Pattern 몇 가지를 정리해본다. `Monolith' 개발 환경에서 일반적으로 아래 디자인 이상으로 복잡한 설계는 비용 낭비라고 생각되며, 결론은 비지니스 로직의 단순화와 도메인 모델의 표현력에 촛점을 맞춘다. 

#### **1. Business Tier by Spring Framework  - #1**

[##\_1C|cfile21.uf.2237E945530199291DDC96.jpg|width="590" height="156" filename="201111291204.PNG" filemime="image/jpeg"|\_##]

1) @Service는 Business Object의 Façade Pattern 역할을 한다.

2) Façade의 Method는 하나의 트랜잭션 단위로 묶일 수 있다.

3) Business Object는 상호 의존성이 없는 재사용이 가능한 독립적인 객체가 된다.

4) Façade는 Business 처리에 대한 상위 API를 노출하는 창구 역할을 한다.

  
  

#### **2. Business Tier by Spring Framework  - #2**

[##\_1C|cfile24.uf.2714233F5301993508361C.jpg|width="590" height="157" filename="201111291205.PNG" filemime="image/jpeg"|\_##]

1) @Service는 Façade Pattern + Business Object Pattern 역할을 한다.

2) Business Object의 Method는 하나의 트랜잭션 단위로 묶일 수 있다.

3) Layer의 단일화로 개발 공수가 적어진다.

4) Business Object 사이의 결합도가 높아져 재사용성이 떨어진다.

ps. 정리되는대로 추가로 몇가지 더 올려볼 생각이다.

