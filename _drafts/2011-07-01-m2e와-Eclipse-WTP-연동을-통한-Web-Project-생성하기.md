---
title: "m2e와 Eclipse WTP 연동을 통한 Web Project 생성하기"
tags: [eclipse, m2e, maven, wtp]
date: 2011-07-01T23:41:00+09:00
---

<font color="#d18e0a">* 작업 환경 : JDK 6, Tomcat 6, Eclipse Indigo<br>
</font>  
  
Eclipse의 Maven 플러그인인 m2e를 통해 생성된 프로젝트는 WTP 플러그인과 연동되지 않아 여러 불편함이 있었다. 하지만 m2e Extra 플러그인을 추가 설치하여 손쉽게 WTP와 연동하고 개발할 수 있는 방법을 제공한다.  
  
(참고로 또다른 이클립스 Maven 플러그인 q4e는 기존 WTP의 Dynamic Web Project를 간편하게 Maven 프로젝트로 변환할 수 있는 기능을 제공한다. 그래서 거의 q4e만 사용했었다...;))

#### **1. m2e Plugin 설치**
Help -\> Install New Software... -\> Work with -\> [http://m2eclipse.sonatype.org/sites/m2e](http://m2eclipse.sonatype.org/sites/m2e)  

[##\_1C|cfile26.uf.261593455301984E2BA0A7.jpg|width="590" height="480" filename="201107162258.jpg" filemime="image/jpeg"|\_##]

  
  

#### **2. m2e Extra Plugin 설치**
Help -\> Install New Software... -\> Work with -\> [http://m2eclipse.sonatype.org/sites/m2e-extras](http://m2eclipse.sonatype.org/sites/m2e-extras)

[##\_1C|cfile23.uf.260FFB415301985C205D2F.jpg|width="590" height="480" filename="201107162309.jpg" filemime="image/jpeg"|\_##]

  
기타 OSGi 개발환경이나 Hudson 등의 연동환경을 제공한다. 여기서는 WTP와의 연동을 위한 플러그인만을 선택한다.  
  
  

#### **3. 프로젝트 생성**
New -\> Project -\> Maven -\> Maven Project  

[##\_1C|cfile7.uf.262066415301986A1896F5.jpg|width="525" height="500" filename="201107162315.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile21.uf.231E944653019877190D73.jpg|width="590" height="528" filename="201107162316.jpg" filemime="image/jpeg"|\_##]

  
Artifact Id : maven-archetype-webapp 선택  

[##\_1C|cfile6.uf.232B3642530198862BD789.jpg|width="590" height="525" filename="201107162317.jpg" filemime="image/jpeg"|\_##]

  
생성할 Maven 프로젝트의 Group Id와 Artifact Id를 원하는 값으로 입력한다.  

[##\_1C|cfile26.uf.271D4E455301989529363E.jpg|width="590" height="525" filename="201106172318.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile27.uf.27537344530198AA15D876.jpg|width="249" height="306" filename="201107162406.jpg" filemime="image/jpeg"|\_##]

- 정상적으로 프로젝트가 생성된 모습 -  
  

  

#### **4. 프로젝트 설정 및 실행**
생성된 프로젝트의 기본 설정은 Java Compiler Level 1.5, Java Dynamic Module 2.3이며, 변경이 필요한 경우 Preference에서 변경하여 준다.  

[##\_1C|cfile22.uf.224A5740530198B60E21BC.jpg|width="405" height="354" filename="201107162415.jpg" filemime="image/jpeg"|\_##]

  
Tomcat으로 기동하여 테스트하여 본다.  
test 프로젝트 선택 -\> Run as -\> Tomcat  

[##\_1C|cfile23.uf.2638AD3E530198C002AB97.jpg|width="317" height="157" filename="201107162417.jpg" filemime="image/jpeg"|\_##]

