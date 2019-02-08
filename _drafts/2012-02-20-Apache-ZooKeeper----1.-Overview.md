---
title: "Apache ZooKeeper  - 1. Overview"
tags: zookeeper
date: 2012-02-20T21:43:16+09:00
---

-Apache Zookeeper-는 분산 코디네이터로 얼마전까지 Hadoop의 서브 프로젝트였다가 apache 메인 프로젝트로 당당히 납시셨다.;;;  
레퍼런스는 [http://zookeeper.apache.org/](http://zookeeper.apache.org/)를 참고하였으며, 현재일자 기준 최신버전인 3.4.2 버전을 토대로 작성하였다.

## 1. 개요
ZooKeeper는 분산환경의 상호 조정에 필요한 다양한 서비스를 제공하는 동기화가 보장되는 분산파일 시스템이다. 따라서 개발자는 ZooKeeper를 이용하여 안정적인 분산 어플리케이션을 개발할 수 있다.  
  

## 2. Data Model
### 2.1. ZNodes
ZooKeeper는 znode라는 계층적인 노드 구조(Tree Structure)의 형태로 데이터를 유지하며, 각각의 znode는 데이터를 저장하고 ACL을 포함한다.

![znode](/assets/image/2012-02-20-201202201201.jpg)

데이터 접근은 원자적이며(Success or Fail), WRITE 연산은 데이터 추가 및 가공이 불가하며 오로지 'replace' 단위로 동작한다. 마찬가지로, READ 연산도 znode 데이터의 일부 필요한 부분이 아닌 전체만 받아올 수 있다.   
  
znode는 절대경로로 참조되며, '.', '..'와 같은 상대경로 표시자는 허용되지 않는다. 또한 'zookeeper'라는 ZooKeeper가 관리 정보를 사용하는 데 필요한 예약된 znode 및 몇가지 제약사항을 제외하고는 모든 경로는 유니코드로 표현될 수 있다.  
  
- 경로명 제약사항  
> The null character (\u0000) cannot be part of a path name. (This causes problems with the C binding.)  
The following characters can't be used because they don't display well, or render in confusing ways: \u0001 - \u0019 and \u007F - \u009F.  
> The following characters are not allowed: \ud800 -uF8FFF, \uFFF0-uFFFF, \uXFFFE - \uXFFFF (where X is a digit 1 - E), \uF0000 - \uFFFFF.  

znode는 클라이언트의 세션과 관계없이 명시적으로 삭제되며, 자식 노드를 가질 수 있다.

  

### 2.2. Ephemeral Nodes (임시 노드)
Ephemeral Node는 클라이언트의 세션이 종료될 때, ZooKeeper에 의해 삭제되는 임시 노드로서, 자식 노드를 가질 수 없다.

### 2.3. Sequence Nodes (순차적 노드)
Sequence Node는 부모 znode가 관리하게 되는 증가 카운터를 통해 znode 경로명의 일부에 순차 번호가 부여되는 노드이다. Sequence Flag를 설정하고 znode를 생성하면, 증가 카운터값이 자식 znode 경로명에 붙여져 생성된다. Sequence Node는 보통 정렬 및 Lock Service에 사용될 수 있다.  

### 2.4. Watches
Watch는 할당된 znode의 변경에 대한 이벤트를 감지하고 클라이언트에 통지한다.  
READ 연산인 exists, getChildren, getData에 Watch가 설정되고, WRITE 연산인 create, setData, delete에서 변경감지 및 통지가 이루어진다.  
(ACL 연산은 Watch 대상이 아님)  
  
- 예 1) 발생 이벤트와 처리  

| Event | Handling |
|-------|----------|
| **NodeCreated, NodeDeleted** |  변경된 znode의 경로를 전달받아 후처리 |
| **NodeChildrenChanged** |  getChildren을 다시 호출하여 갱신된 노드리스트를 획득하고 후처리 |
| **NodeDataChanged** |  getData를 다시 호출하여 갱신된 데이터를 획득하고 후처리 |
  
  

## 3. Sessions
클라이언트가 분산 서버 중 한곳과 연결이 성공하면, 서버는 타임아웃 값을 가지고 클라이언트와의 세션을 생성한다.  
만약 서버가 타임아웃 시간동안 클라이언트로부터 응답을 받지 못하면, 세션에 관계된 모든 Ephemeral Nodes가 제거되고 세션은 종료된다.  
클라이언트는 백그라운드에서 특정 주기로 ping을 요청하여 서버와의 세션을 유지한다.

![zk sessions](/assets/image/2012-02-20-201202201151.jpg)

## 4. Failover
클라이언트에 의해 자동으로 일어나며, 장애가 일어난 서버로부터 세션 및 세션에 연관된 znode의 정보도 모두 새로이 연결된 서버로 동기화된다.  
  

## 5. ACL
ACL은 어떤 클라이언트가 특정 연산을 수행할 수 있는지 결정하는 인증정보이다.  
  
예 1) ACL 인증  

| Type | Description |
|-------|----------|
| **digest** | Username/Password 인증 |
| **host** | 호스트명 인증 |
| **ip** | IP 인증 |

  
예 2) ACL 권한  

| Permission | Method |
|-------|----------|
| **CREATE** | create(Child Node) |
| **READ** | getChildren(), getData() |
| **WRITE** | setData() |
| **DELETE** | delete(Child Node) |
| **ADMIN** | setACL() |

- 자세한 내용은 API Document의 ZooDefs.Ids 상수 참조.
  
  

## 6. 연산

| Ops | Description |
|-------|----------|
| **create** | znode 생성 |
| **delete** | znode 삭제 |
| **exists** | znode 존재 여부 |
| **getACL, setACL** | znode의 ACL 정보 획득 및 설정 |
| **getChildren** | znode의 자식 노드리스트 획득 |
| **getData, setData** | znode 데이터 획득 및 저장 |
| **sync** | 분산 환경에서 ZooKeeper 마스터 서버와의 데이터 동기화 |

ZooKeeper의 모든 변경 연산은 Non-blocking으로 동작한다.  
그리고 동일한 연산에 대한 동기/비동기 방식을 지원한다.  
  
예 1) ACL 정보 획득 API  
```java
// 동기방식
public List<ACL> getACL(String path, Stat stat);

// 비동기방식
// Callback Object를 통한 결과 recieve.
public void getACL(String path, Stat stat, AsyncCallback.ACLCallback cb, Object ctx);
```
  

## 7. Status
getState 호출을 통해 전달받는 ZooKeeper.States 상수로 ZooKeeper 상태를 확인할 수 있다.  
  
- ZooKeeper.States Constants  
```
ASSOCIATING  
AUTH_FAILED  
CLOSED  
CONNECTED  
CONNECTEDREADONLY  
CONNECTING  
NOT_CONNECTED  
```

## 8. Ensemble
복제 모드로 수행되는 ZooKeeper 클러스터링 서버 구성을 말한다.  
Ensemble 내에서는 과반수 이상의 서버가 운영되어야만 서비스가 가능하므로 보통 홀수개의 서버로 구성한다.

