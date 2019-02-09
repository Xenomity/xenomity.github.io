---
title: "JDK 7 New Features"
tags: java
date: 2011-04-01T11:17:58+09:00
---

오라클은 Lambda, Jigsaw, Coin의 일부 기능을 제거한 JDK 7를 2011년 중반에 출시하고 나머지 기능 + Alpha를 2012년에 JDK 8으로 출시한다는 계획이다.  
  
  
## G1 Garbage First Collector  
서버측 JVM에서 주로 사용되던 CMS (Concurrent Mark-Sweep) collector를 대신할 새로운 방식의 collector. 기존 방법보다 GC에 의한 pause time을 줄이고 JVM 성능에 대한 예측을 더 정확하게 할 수 있도록 한다. 이 기능은 실험적으로 Java SE 6 u14 버전에 포함되었다.  
  
  
## Java외의 다른 언어 지원 (invokeDynamic)  
이미 Java VM에서 실행되는 여러 개발언어가 있다. JDK 7은 Java 언어 외 다른 개발언어도 Java 언어와 거의 같은 성능으로 Java VM에서 수행될 수 있도록 한다.  
JVM은 사실 java 언어와는 아무런 연관이 없다. JVM은 언어에 대해서는 아무것도 모르며 단지, byte 코드와 기타 정보를 가진 class 파일 포맷에 대해서만 알고 있다는 것이다. Java 언어가 아니라도 JVM 스팩에서 정의한 클래스 포맷과 byte 코드 명령어 규칙을 따르기만 하면 JVM에서 실행할 수 있다.  
  
### 현재 JDK 6에서 지원하고 있는 외부 언어
- Jruby – Java로 구현한 Ruby
- Jython – java로 구현한 Python
- Groovy – JVM에서 실행되는 새로운 동적 스크립트 언어
- Rhino – Java로 구현한 JavaScript
- JavaFX – Oracle이 RIA를 위해 새로 만든 언어
- Mirah – Ruby와 유사한 문법의 정적 타입 언어
  
### 기존 JVM에서의 동적 스크립트 언어 지원 문제  
Java는 정적형 언어다. 객체든 변수든 사용하기 위해서는 프로그램 코드에 type을 지정해 주어야 한다. 어떤 메쏘드를 호출하기 위해서는 메쏘드를 가진 객체, 메쏘드의 인자 값, 반환 값까지 어떤 Object Type인지 알고 있어야 한다. 이와 달리 Ruby와 같은 동적 스크립트 언어는 메쏘드를 호출하기 위해 객체의 Type이 무엇인지 몰라도 상관없다.  
JDK 7의 JVM에서는 `invokedynamic` 명령어를 추가하여 동적 스크립트 언어가 Java 언어와 거의 같은 성능을 낼 수 있도록 한다. `invokedynamic`은 기존의 `invokevirtual`, `invokeinterface`와 달리 메서드 호출을 위한 클래스의 정보가 필요하지 않다. 호출할 메서드의 이름과 선언 형식만 제공하면 런타임 시점에에 적절한 메서드를 호출할 수 있다.  
  
  
## Project Coin  
JDK 7은 Java 프로그래밍 언어에 코딩을 편하게 하는 기능들을 제공한다. Project Coin은 JDK 7에 추가될 작은 언어(Java)적 변화를 결정하는 프로젝트다.  
  
### Strings in switch  
- 현재
```java
String str = ...  
  
if (str.equals("name")) {  
    } else if (str.equals("surname")) {  
    } else if (str.equals("forename")) {  
}
```

- JDK 7
```java
String str = ...  
  
switch (str) {  
    case "name":  
        break;  
    case "surname":  
        break;  
    case "forename":  
        break;  
}
```
  
### Map for-each
- 현재  
```java
Map<String, Integer> map = new HashMap<String, Integer>();  
  
for (Entry<String, Integer> entry : map.entrySet()) {  
    System.out.println(entry.getKey() + "=" + entry.getValue());  
}
```

- JDK 7
```java
Map<String, Integer> map = new HashMap<String, Integer>();  

for (String key, Integer value : map) {
    System.out.println(key + "=" + value);
}
```
 
### For-each iteration control
- 현재  
```java
List<String> list = new ArrayList<String>();

for (String str : list) {
    if (str.length() > 100) {
        // str 변수에 할당된 문자열을 제거.
    }
}
```

- JDK 7  
```java
List<String> list = new ArrayList<String>();

for (String str : list : it) {
    if (str.length() > 100) {
        it.remove();
    }
}
```
  
### List/map access
- 현재
```java
List<String> list = ...  
Map<String, Integer> map = ...  
Map<String, Map<String, Task>> combined = ...  

String str = list.get(0);
list.set(0, "Hi");

Integer value = map.get(str);
map.put("Hi", 56);

Task task = combined.get("Test").get("Monitor");
```

- JDK 7  
```java
List<String> list = ...
Map<String, Job> map = ...
Map<String, Map<String, Task>> combined = ...

String str = list[0];
list[0] = "Hi";

Integer value = map[str];  
map["Hi"] = 56;  
  
Task task = combined["Test"]["Monitor"];
```
  
### Infer generics in declarations
- 현재
```java
List<String> list = new ArrayList<String>();  
Map<String, Job> map = new HashMap<String, Job>();  
Map<String, Map<Channel, Job>> map = new HashMap<String, <Channel, Job>>();
```

- JDK 7  
```java
List<String> list = new ArrayList<>();  
Map<String, Job> map = new HashMap<>();  
Map<String, Map<Channel, Job>> map = new HashMap<>();
```
  
### String interpolation
- 현재
```java
String name = ...  
int value = ...  
String out = "The value of "+ name + " is " + value;
```

- JDK 7  
```java
String name = ...  
int value = ...  
String out = $"The value of ${name} is ${value}";
```
  
### Multi-catch of Exceptions
- 현재
```java
try {  
    doWork(file);  
} catch (IOException ioe) {  
    logger.log(ioe);  
    throws ioe;  
} catch (SQLException sqle) {  
    logger.log(sqle);  
    throws sqle;  
}
```

- JDK 7  
```java
try {  
    doWork(file);  
} catch ( final IOException | SQLException ex ) {  
    logger.log(ex);  
    throws ex;  
}
```
  
### Multi-line Strings
- 현재  
```java
String sql = "SELECT surname, forename, title " +  
        "FROM person";
```

- JDK 7  
```java
String sql = """SELECT surname, forename, title   
        FROM person""";
```
  
### Resource management
- 현재  
```java
File file = ...  
FileReader in = null;  
  
try {  
    in = new FileReader(file);  
    processFile(in);  
} finally {  
    try {  
        if (in != null) {  
            in.close();  
        }  
    } catch (IOException ignored) {}  
}
```

- JDK 7  
```java
File file = ...  
  
try (FileReader in = new FileReader(file)) {  
    processFile(in);  
}
```
  
### Null-handling
- 현재  
```java
Session sess = ...  
String code = null;  
  
if (sess != null) {  
    if (sess.person() != null) {  
        if (sess.person().address() != null) {  
            code = sess.person().address().postcode();  
        }  
    }  
}
```

- JDK 7  
```java
Session sess = ...  
String code = sess?.person()?.address()?.postcode(); |
```
  
  
## Lambda (Closure) 지원  
요즘 사용하는 대부분의 desktop, laptop 컴퓨터는 이미 멀티코어 CPU가 기본으로 사용되고 있다. 앞으로는 모바일 장치에서도 멀티코어 CPU가 사용될 수도 있을 것이다. Java 개발자들이 손쉽게 멀티 코어 CPU의 이점을 활용할 수 있도록 JDK 7에서는 Closure를 지원할 예정이다.

- For loop – External Iteration  
```java
double highestScore = 0.0;  
  
for (Student s : students) {  
    if ((s.gradYear == 2010) {  
        if(s.score > highestScore)){  
            highestScore = s.score;  
        }  
    }  
}
```

- Hypothetical Internal Iteration  
```java
double highestScore = students.filter (new Predicate<Student>() {  
    public boolean isTrue(Student s) {  
        return s.gradYear == 2010;  
    }  
}).map (new Extractor<Student, Double>() {  
    public Double extract(Student s) {  
        return s.score;  
    }  
}).max();  
```

- Introducing Lambda Expressions  
```java
double highestScore = students  
        .filter(#{ Student s -> s.gradYear == 2010 } )  
        .map(#{ Student s -> s.score } ).max();
```
  
  
## SDP, SCTP 프로토콜 지원  
Telco에서 많이 사용하는 SDP (Sockets Direct Protocol), SCTP (Stream Control Transmission Protocol)을 지원한다.  
  
  
## ECC (Ecliptic Curve Cryptography)  
RSA 알고리즘보다 더 효율적인 ECC 알고리즘을 제공한다.  
  
  
## 새로운 Swing Look & Feel - Nimbus  
Swing Look & Feel에 Nimbus L&F을 추가로 제공한다. Nimbus L&F은 정적인 비트맵을 사용하지 않고 2D 그래픽 API로 구현되어 어떤 해상도에서도 최적의 UI를 보여준다.  
  
