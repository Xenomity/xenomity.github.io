---
title: "Equinox config.ini 기본 내용"
tags: [eclipse, equinox, osgi, rcp]
date: 2010-12-27T16:05:37+09:00
---

Equinox Bundle의 config.ini 환경설정 파일은 {workspace}/configurations 경로에 최초실행시, 자동으로 생성된다.

* * *

eclipse.ignoreApp=[true|false] (Equinox의 경우에만 해당)  
osgi.bundles=자동으로 install할 번들의 나열.  
osgi.console=원격으로 오픈할 포트.  
osgi.bundles.defaultStartLevel=정수 [default:4] 번들의 기본 StartLevel.  
osgi.startLevel=정수 [default:6] 정해진 레벨까지의 번들만이 시작된다.  
osgi.clean=[true|false] 저장영역 초기화  
  
