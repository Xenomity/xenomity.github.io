---
title: "Overview MapReduce"
tags: [hadoop, mapreduce]
date: 2011-11-01T18:07:38+09:00
---

MapReduce는 Google에서 정보 검색을 위한 Big Data의 가공을 목적으로 개발된 분산 환경에서의 병렬 데이터 처리 기법이자 프로그래밍 모델이며, 이를 지원하는 시스템을 통틀어 일컫는다. 줄여서 M/R이라고도 하며, LISP 언어에서의 map()과 reduce() 함수의 개념을 차용하여 분산 시스템의 구조를 감추면서 범용 언어(Java, C++ 등)로의 구현을 용이하게 한다.

## 1. Pros
1) Map, Reduce라는 함수의 구현을 통해 병렬처리 로직 없이 손쉽게 병렬 데이터 처리를 할 수 있다.  
2) 특정한 데이터 모델이나 질의 언어에 비의존적이다.  
3) 저장 시스템(파일시스템, DBMS 등)에 독립적이다.  
4) low-performance PC를 Node에 추가 할당함으로써 유연한 확장이 가능하다. 할당된 Node의 시스템을 업그레이드하기보단, 분산 처리를 담당할 추가 PC를 구성하는 것이 용이하다.  
  

## 2. Cons
1) DBMS에 비해 일반적으로 낮은 성능을 보인다.  
2) 스케줄링 정책이 단순하다.  
3) 여러 3rd party, 개발 도구들의 지원이 미비하다.  
  

## 3. MapReduce 구현체
대표적인 OpenSource Framework로 Apache Hadoop이 있다.  
[http://hadoop.apache.org/](http://hadoop.apache.org/)  
   
Hadoop의 Overview를 잠깐 살펴보자면,  

> The Apache™ Hadoop™ project develops open-source software for reliable, scalable, distributed computing. 

> The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using a simple programming model. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-avaiability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-availabile service on top of a cluster of computers, each of which may be prone to failures.

  
  
ps. 참고로 Hadoop 관련 프로젝트들의 이름이 재밌다.ㅋ  

| Project | Description |
|---------|-------------|
| **Avro™** | 데이터 직렬화 시스템 |
| **Cassandra™** | 확장 가능한 다중 마스터 데이터베이스 |
| **Chukwa™** | 분산 시스템 관리 데이터 수집 시스템 |
| **HBase™** | 대형 테이블에 대한 구조화된 데이터 스토리지를 지원하는 확장/분산 데이터베이스 |
| **Hive™** | 데이터웨어 하우스 |
| **Mahout™** | 확장 가능한 데이터 마이닝 라이브러리 |
| **Pig™** | 높은 수준의 데이터 흐름 언어와 병렬 계산을 위한 프레임워크 |
| **ZooKeeper™** | 분산 어플리케이션을 위한 고성능 조정 서비스 |

  
