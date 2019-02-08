---
title: "m2e와 Eclipse WTP 연동을 통한 Web Project 생성하기"
tags: [eclipse, m2e, maven, wtp]
date: 2011-07-01T23:41:00+09:00
---

* 작업 환경: JDK 6, Tomcat 6, Eclipse Indigo
  
Eclipse의 Maven 플러그인인 m2e를 통해 생성된 프로젝트는 WTP 플러그인과 연동되지 않아 여러 불편함이 있었다. 하지만 m2e Extra 플러그인을 추가 설치하여 손쉽게 WTP와 연동하고 개발할 수 있는 방법을 제공한다.  
  
참고로 또다른 이클립스 Maven 플러그인 q4e는 기존 WTP의 Dynamic Web Project를 간편하게 Maven 프로젝트로 변환할 수 있는 기능을 제공한다.

## 1. m2e Plugin 설치
Help -> Install New Software... -> Work with -> [http://m2eclipse.sonatype.org/sites/m2e](http://m2eclipse.sonatype.org/sites/m2e)  

![step 1](/assets/image/2011-07-01-201107162258.jpg)
  
  

## 2. m2e Extra Plugin 설치
Help -\> Install New Software... -\> Work with -\> [http://m2eclipse.sonatype.org/sites/m2e-extras](http://m2eclipse.sonatype.org/sites/m2e-extras)

![step 2](/assets/image/2011-07-01-201107162309.jpg)
  
기타 OSGi 개발환경이나 Hudson 등의 연동환경을 제공한다. 여기서는 WTP와의 연동을 위한 플러그인만을 선택한다.  
  
  

## 3. 프로젝트 생성
New -> Project -> Maven -> Maven Project  

![step 3](/assets/image/2011-07-01-201107162315.jpg)
  
![step 4](/assets/image/2011-07-01-201107162316.jpg)
  
Artifact Id: maven-archetype-webapp 선택  

![step 5](/assets/image/2011-07-01-201107162317.jpg)
  
생성할 Maven 프로젝트의 Group Id와 Artifact Id를 원하는 값으로 입력한다.  

![step 6](/assets/image/2011-07-01-201106172318.jpg)
  
![step 7](/assets/image/2011-07-01-201107162406.jpg)

- 정상적으로 프로젝트가 생성된 모습 -  
  

  

## 4. 프로젝트 설정 및 실행
생성된 프로젝트의 기본 설정은 Java Compiler Level 1.5, Java Dynamic Module 2.3이며, 변경이 필요한 경우 Preference에서 변경하여 준다.  

![step 8](/assets/image/2011-07-01-201107162415.jpg)
  
Tomcat으로 기동하여 테스트하여 본다.  
test 프로젝트 선택 -> Run as -> Tomcat  

![step 9](/assets/image/2011-07-01-201107162417.jpg)
