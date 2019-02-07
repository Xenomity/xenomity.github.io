---
title: "Java String Pool"
tags: [java]
date: 2010-03-08T17:07:00+09:00
---

Java는 8가지 Primitive Type을 제외하고는 전부 Object Type이다. 그러나, 자주 사용하는 문자열의 사용 편의성을 위해, 문법차원에서의 방법도 제공한다.

```
String str1 = "abc"; String str2 = "abc"; String str3 = new String("abc");
```

객체를 생성하는 방법(new)이 아닌 변수 대입 방법이 이것인데,  
  
1- "abc"라는 문자열을 String Pool에서 검색하고, 존재하지 않으면 새로운 String Instance를 생성하며 존재하는 경우 기존 객체를 레퍼런싱한다. 즉, 여기서는 새로운 객체가 메모리에 생성된다.  
2- 이미 "abc"라는 문자열이 String Pool에 존재하므로, str1의 메모리 주소를 동일하게 참조한다.  
3- 무조건 새로운 문자열 객체를 생성한다.  
  
