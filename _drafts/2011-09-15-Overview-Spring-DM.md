---
title: "Overview Spring DM"
tags: [osgi, spring, spring-dm]
date: 2011-09-15T17:42:07+09:00
---

OSGi에 대한 내용들은 이전에 포스팅한 글들을 참고한다. (Search -\> OSGi)  
  
Spring DM에 대한 간략한 Overview가 필요하여 정리하였다.

## 1. OSGi의 특징
Dynamic Module System for Java.  
Bundle과 Service의 통합 및 관리.  
Runtime 시점에서의 Bundle (Service) 상태 변화.  


## 2. OSGi 적용 사례
Eclipse Platform (Equinox), Lotus, Websphere App-Server, SpringSource dmServer 등등...  


## 3. OSGi Framework
OSGi Specification의 구현체.  
OpenSource로는 Eclipse Equinox, Apache Felix, Knopflerfish 등이 있으며, 상용으로는 ProSyst, Knopflerfish Pro 등이 있다.  


## 4. OSGi Bundle
Bundle은 Module이라고도 하며, OSGi Framework는 API의 가시성, Bundle간의 의존성, Bundle의 버전 등을 정의하고 Life-cycle을 관리한다.   
Runtime 시점에 OSGi Framework는 Bundle의 상태를 동적으로 변화시킬 수 있다.  
  
자세한 내용은 [이전 포스팅](http://blog.xenomity.com/OSGi-Bundle-Life-cycle) 참고.  


## 5. OSGi Service
Runtime 시점에 OSGi Framework는 Service를 Service Registry에 등록/해제시킬 수 있다.  
Bundle은 Service Registry에서 Service를 획득/반납할 수 있다.  


## 6. 일반 OSGi Framework의 단점
OSGi Framework의 API에 종속적인 코드가 작성된다.  
Service는 개발자의 Code-level에서 관리된다.  
Enterprise 환경 지원이 어렵다.  
 

## 7. Spring DM (Dynamic Module)의 특징
위 6번의 단점들을 커버하고 나온 기특한 녀석..ㅡ.ㅡ;;;  
다양한 OSGi Framework와의 연동이 가능하며, OSGi Framework 위에서 Spring-based Enterprise Application의 개발이 가능하다. (Spring DM web support)  


## 8. Spring DM의 장점
설정만으로 POJOs to OSGi.  
Spring Bean과 OSGi Service의 통합.  
OSGi Container, OSGi API 의존성이 불필요한 아주~ 편한 Test 가능.  
Spring Framework의 모든 추상적 기능을 그대로 사용할 수 있음.  
 

## 9. Spring OSGi Model
![spring osgi model](/assets/image/2011-09-15-201109151738.jpg)

- gerd-wuetherich.de에서 발췌
  
  
## 10. Spring DM Bundle
Bundle의 상태에 따라 Spring DM은 ApplicationContext를 생성하고 소멸시킨다.  
Bundle은 MANIFEST.MF의 Spring-Context Header 또는 XML 파일로 정의한다.  
 

## 11. Spring DM Service
Spring Bean들은 OSGi의 Service로 내보낼 수 있다.  
반대로 이미 노출된 OSGi Service도 Spring Bean으로 ApplicationContext에 정의하여 사용할 수 있다.  
`<osgi:service>`, `<osgi:reference>` Tag를 통해 정의.  
고로, Spring Bean은 OSGi Service라고 볼 수 있다.  


## 12. 일반 OSGi에서의 Web Application
1) Web Application 위에서 OSGi Framework의 실행.  
2) OSGi Framework 위에서 Bundle처럼 Web Container의 실행.  


## 13. Spring DM에서의 Web Application (Spring DM Web Support)
WAR를 Bundle화하여 OSGi Framework으로 Bundle Install.  
WAR Bundle 시작시 Web Extender를 통해 Web Container에 Web Application 설치.  

![bundle install](/assets/image/2011-09-15-201109161011.jpg)

- gerd-wuetherich.de에서 발췌
  

OsgiBundleXmlWebApplicationContext와 Spring의 ContextLoaderListener를 통한 연동.  
예) web.xml  
```xml
...
 
<context-param>
    <param-name>contextClass</param-name>
    <param-value>org.springframework.osgi.web.context.
        support.OsgiBundleXmlWebApplicationContext</param-value>
</context-param>
 
<listener>
    <listener-class>
        org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
 
...
```

