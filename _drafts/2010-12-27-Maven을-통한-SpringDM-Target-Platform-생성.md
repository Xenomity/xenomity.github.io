---
title: "Maven을 통한 SpringDM Target Platform 생성"
tags: [maven, osgi, spring, spring-dm]
date: 2010-12-27T15:41:38+09:00
---

\* Spring-OSGi (DM) v1.2 기준으로 작성해봤다.  
  
- 기본 스펙  
Jetty Container v6.1  
Equinox Framwork v3.4  
Spring 2.5.6a  
Spring DM v1.2.0  
  
- 작업 환경  
Eclipse v3.5  
Apache Maven v2.1  
Eclipse IAM (Q4E Plug-in)  
  
  

#### **1. Target Platform을 위한 project를 하나 생성한다.**
  New -\> Plug-in Project  

![Step 1](/assets/image/2010-12-27-201011231335.jpg)
  
Target Platform =\> an OSGi framework =\> Equinox 

Next  

![Step 2](/assets/image/2010-12-27-201011231337.jpg)
  

Target Platform을 위한 Project이므로, OSGi Bundle Activator가 불필요하다. 

따라서 체크 해제.

Finish  

![Step 3](/assets/image/2010-12-27-201011231340.jpg)
  

#### **2. 생성된 프로젝트에 'target' directory 생성.**

  New =\> Folder  
    

#### **3. Maven용 pom.xml 파일을 추가.**

* * *

    \<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemalocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4\_0\_0.xsd"\>   \<modelversion\>4.0.0\</modelversion\>   \<groupid\>com.acme.springdm\</groupid\>   \<artifactid\>com.acme.springdm.target\</artifactid\>   \<packaging\>pom\</packaging\>   \<name\>SpringDM with Jetty\</name\>   \<version\>1.0.0\</version\>   \<description\>   SpringDM Target Platform \</description\> \<properties\>   \<taget-platform.root\>.\target\</taget-platform.root\>   \<equinox.ver\>3.4.2.R34x\_v20080826-1230\</equinox.ver\>   \<springdm.ver\>1.2.0\</springdm.ver\>   \<spring.ver\>2.5.6.A\</spring.ver\> \</properties\> \<dependencies\>   \<dependency\>    \<groupid\>org.eclipse.osgi\</groupid\>    \<artifactid\>org.eclipse.osgi\</artifactid\>    \<version\>${equinox.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.eclipse.osgi\</groupid\>    \<artifactid\>services\</artifactid\>    \<version\>3.1.200-v20070605\</version\>   \</dependency\>     \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-core\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-extender\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-io\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-mock\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-test\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-web\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>spring-osgi-web-extender\</artifactid\>    \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>     \<groupid\>org.springframework.osgi\</groupid\>     \<artifactid\>spring-osgi-annotation\</artifactid\>     \<version\>${springdm.ver}\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.springframework\</groupid\>      \<artifactid\>org.springframework.core\</artifactid\>    \<version\>${spring.ver}\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.springframework\</groupid\>      \<artifactid\>org.springframework.context\</artifactid\>    \<version\>${spring.ver}\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.springframework\</groupid\>      \<artifactid\>org.springframework.web\</artifactid\>    \<version\>${spring.ver}\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.springframework\</groupid\>      \<artifactid\>org.springframework.web.servlet\</artifactid\>    \<version\>${spring.ver}\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.apache.log4j\</groupid\>    \<artifactid\>com.springsource.org.apache.log4j\</artifactid\>    \<version\>1.2.15\</version\>   \</dependency\>   \<dependency\>      \<groupid\>net.sourceforge.cglib\</groupid\>      \<artifactid\>com.springsource.net.sf.cglib\</artifactid\>      \<version\>2.1.3\</version\>   \</dependency\>   \<dependency\>      \<groupid\>javax.servlet\</groupid\>      \<artifactid\>com.springsource.javax.servlet\</artifactid\>      \<version\>2.5.0\</version\>   \</dependency\>   \<dependency\>    \<groupid\>javax.servlet\</groupid\>    \<artifactid\>com.springsource.javax.servlet.jsp\</artifactid\>    \<version\>2.1.0\</version\>   \</dependency\>   \<dependency\>      \<groupid\>javax.el\</groupid\>      \<artifactid\>com.springsource.javax.el\</artifactid\>      \<version\>1.0.0\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.aopalliance\</groupid\>      \<artifactid\>com.springsource.org.aopalliance\</artifactid\>      \<version\>1.0.0\</version\>   \</dependency\>   \<dependency\>      \<groupid\>org.objectweb.asm\</groupid\>    \<artifactid\>com.springsource.org.objectweb.asm\</artifactid\>      \<version\>2.2.3\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>jstl.osgi\</artifactid\>    \<version\>1.1.2-SNAPSHOT\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>jasper.osgi\</artifactid\>    \<version\>5.5.23-SNAPSHOT\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>jasper.osgi\</artifactid\>    \<version\>5.5.23-SNAPSHOT\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>commons-el.osgi\</artifactid\>    \<version\>1.0-SNAPSHOT\</version\>   \</dependency\>   \<!-- Jetty Web Server --\>   \<dependency\>    \<groupid\>org.mortbay.jetty\</groupid\>    \<artifactid\>com.springsource.org.mortbay.jetty.server\</artifactid\>    \<version\>6.1.9\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>jetty.start.osgi\</artifactid\>    \<version\>1.0.0\</version\>   \</dependency\>   \<dependency\>    \<groupid\>org.springframework.osgi\</groupid\>    \<artifactid\>jetty.web.extender.fragment.osgi\</artifactid\>    \<version\>1.0.1\</version\>   \</dependency\> \</dependencies\> \<repositories\>   \<repository\>    \<id\>spring-maven-milestone\</id\>    \<name\>Springframework Maven Repository\</name\>    \<url\>http://s3.amazonaws.com/maven.springframework.org/milestone\</url\>   \</repository\>   \<repository\>    \<id\>spring-osgified-artifacts\</id\>    \<snapshots\>\<enabled\>true\</enabled\>\</snapshots\>    \<name\>Springframework Maven OSGified Artifacts Repository\</name\>    \<url\>http://s3.amazonaws.com/maven.springframework.org/osgi\</url\>   \</repository\>   \<repository\>      \<id\>com.springsource.repository.bundles.release\</id\>      \<name\>SpringSource Enterprise Bundle Repository - SpringSource Bundle Releases\</name\>    \<url\>http://repository.springsource.com/maven/bundles/release\</url\>   \</repository\>   \<repository\>      \<id\>com.springsource.repository.bundles.external\</id\>      \<name\>SpringSource Enterprise Bundle Repository - External Bundle Releases\</name\>      \<url\>http://repository.springsource.com/maven/bundles/external\</url\>   \</repository\>   \<repository\>    \<id\>eclipse-repository\</id\>    \<name\>Eclipse Repository\</name\>    \<url\>http://repo1.maven.org/eclipse/    \</url\>   \</repository\> \</repositories\> \<build\>   \<plugins\>    \<plugin\>     \<groupid\>org.apache.maven.plugins\</groupid\>     \<artifactid\>maven-dependency-plugin\</artifactid\>     \<executions\>      \<execution\>       \<id\>copy-dependencies\</id\>       \<phase\>package\</phase\>       \<goals\>        \<goal\>copy-dependencies\</goal\>       \</goals\>       \<configuration\>        \<outputdirectory\>         ${taget-platform.root}        \</outputdirectory\>        \<overwritereleases\>false\</overwritereleases\>        \<overwriteifnewer\>true\</overwriteifnewer\>       \</configuration\>      \</execution\>     \</executions\>    \</plugin\>   \</plugins\> \</build\>   \</project\>

* * *

![Step 4](/assets/image/2010-12-27-201011231358.jpg)
  

#### **4. Maven Project로 변환.**

Project 선택 후, 우클릭 =\> Maven 2 =\> Use Maven Dependency Management  

![Step 5](/assets/image/2010-12-27-201011231416.jpg)

Maven Project로 변환된 프로젝트 구조
  

#### **5. 라이브러리를 target 경로로 인스톨.**

Project 선택 후, 우클릭 =\> Maven 2 =\> Locally Install Artifact  

![Step 6](/assets/image/2010-12-27-201011231418.jpg)

정상적으로 target directory로 install된 libraries  
  

#### **6. Target Definition**
Project 선택 후, 우클릭 =\> New =\> Other =\> Plug-in Development =\> Target Definition  

![Step 7](/assets/image/2010-12-27-201011231420.jpg)
  
springDM.target이라는 파일이 target directory에 생성되며, 편집창이 자동으로 열리게 된다.  

![Step 8](/assets/image/2010-12-27-201011231422.jpg)
  
Add =\> Directory =\> Next  

![Step 9](/assets/image/2010-12-27-201011231423.jpg)

Browse =\> 현재 프로젝트의 target 전체경로를 선택
  

![Step 10](/assets/image/2010-12-27-201011231426.jpg)

정상적으로 bundle이 추가된 화면
  

#### **7. Target Platform 설정.**
마지막으로 저장(Save)을 하고, Windows 메뉴 =\> Preference =\> Plug-in Development =\> Target Platform을 선택한다.  

![Step 11](/assets/image/2010-12-27-201011231427.jpg)

위에서 생성한 product를 선택하고 Apply(적용)

Enjoy SpringDM~ Enjoy OSGi!!!

