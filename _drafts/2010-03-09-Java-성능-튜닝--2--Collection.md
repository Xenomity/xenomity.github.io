---
title: "Java 성능 튜닝 -2- Collection"
tags: [java]
date: 2010-03-09T09:08:00+09:00
---

Java에서는 Map, List, Set의 자료구조를 구현한 다양한 Collection Framework를 표준 패키지로 제공한다. 대표적으로 자주 사용되는 Map, List의 비교표를 보자면,

| Interface | Class      | Synchronized |
|-----------|------------|--------------|
| Map       | HashMap    | X | 
|           | Hashtable  | O | 
|           | TreeMap    | X | 
| List      | ArrayList  | X |  
|           | Vector     | O |
|           | Stack      | O |  
|           | LinkedList | X | 
  
기본적으로 단일 쓰레드의 경우, 자료구조가 동일한 경우에는 `Hashtable`보다는 `HashMap`을 사용하는것이 훨씬 빠르며, 역시 `Vector`보다는 `ArrayList`를 사용하는 것이 좋다.  
  

#### **1. Vector Capacity**
Vector의 Default Capacity는 10이며, Element의 증가시마다 기존 크기의 2배 크기의 새로운 배열을 할당하고 이전 배열은 버려진다. 데이터의 크기를 어느정도 예측할 수 있다면, 충분한 초기값을 지정해주는 것이 성능을 높히는 효율적인 코딩습관이 될 수 있다.  

```
Vector\<E\> vector = new Vector\<E\>(10000);
```
  
예) 10,000건의 데이터를 100번의 루프를 통해 Vector에 삽입하여 소요된 시간  
```
// 1,033ms - 초기값 10, 증가값 10
new Vector();

// 3,882ms - 초기값 100, 증가값 20
new Vector(100, 20);

// 402ms - 초기값 10000, 증가값 10000
new Vector(10000);
```

#### **2. Hashtable/HashMap Capacity**
Hashtable/HashMap의 Default Capacity는 101, Default Load Factor는 0.75이다. 여기서 Load Factor는 Capacity가 얼마만큼 할당되었을 때 re-hashing(Capacity를 두배로 늘림)할 것인가의 비율을 의미한다. 일반적으로 Load Factor의 값은 합리적인 비율이라고 하며, 충분한 초기값을 주어 가능하면 re-hashing이 일어나지 않도록 하는 것이 관점이다. re-hashing은 상당한 비싼 작업 중 하나이므로...  
  
```
Map\<K, V\> map = new HashMap\<K, V\>(100);
```
  
위와 같이 HashMap을 생성하면 Capacity는 100, Load Factor는 0.75가 되어 map에 75건의 Element가 적재되면 자동으로 re-hashing이 일어나게 된다. 계산하해보면, 예측 가능한 Element 개수의 1.33배 이상으로 Capacity를 지정해주면 re-hashing이 일어나지 않게 할 수 있다는 답이 나온다.  
  
예) 10,000건의 데이터 적재시  
```
// 10000 x 1.33 = 13300
new HashMap\<K, V\>(13300);
```
  
