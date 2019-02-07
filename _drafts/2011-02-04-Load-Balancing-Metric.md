---
title: "Load Balancing Metric"
tags: infrastructure
date: 2011-02-04T14:08:00+09:00
---

로드 밸런싱의 기법이나 알고리즘은 다양하지만, 일반적으로 많이 사용되는 아이들이다.

| 

 Least Connection 

 | 

 활성 세션이 가장 적은 쪽으로 세션 전달.

 |
| 

 Round-Robin

 | 

 순차적 세션 전달.

 |
| 

 Hash

 | 

 동일 서버로 세션 전달.

 |
| 

 Response Time

 | 

 응답 시간에 대한 학습을 통해, 빠른 쪽으로 많은 세션을 전달하고, 느린 쪽으로 적은 세션을 전달.

 |

