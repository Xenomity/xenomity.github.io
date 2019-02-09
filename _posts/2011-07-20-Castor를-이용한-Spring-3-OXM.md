---
title: "Castor를 이용한 Spring 3 OXM"
tags: [castor, oxm, spring, xml]
date: 2011-07-20T12:30:00+09:00
---

Spring OXM을 이용하면 거의 대다수의 OXM 프레임워크들의 일관된 적용이 가능하다. 그 중 Castor는 별도의 매핑정보가 없이도 심플하게 XML-Object간 Marshalling/Unmarshalling이 가능한 Mapper로서, `ContentNegotiatingViewResolver` 테스트중에 잠시 작성해 보았다.

## 1. Castor 추가
현재일자 기준으로 최신버전은 1.2이다.  
  
- Maven pom.xml

```xml
<!-- Castor -->
<dependency>
    <groupId>org.codehaus.castor</groupId>
    <artifactId>castor</artifactId>
    <version>1.2</version>
</dependency>
<dependency>
    <groupId>xerces</groupId>
    <artifactId>xercesImpl</artifactId>
    <version>2.8.1</version>
</dependency>
```
  

## 2. MarshallingView 등록
Spring Context에 `MarshallingView`를 등록할 때, 디폴트 marshaller로 `CastorMarshaller`를 적용시켜 준다. 아래는 `ContentNegotiatingViewResolver`를 통한 `MarshallingView` 등록의 예이다.  
  
- applicationContext.xml

```xml
...
 
<!-- ViewResolver -->
<bean
  class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
  <property name="mediaTypes">
    <map>
      <entry key="html" value="text/html" />
      <entry key="json" value="application/json" />
      <entry key="xml" value="application/xml" />
    </map>
  </property>
  <property name="defaultViews">
    <list>
      <bean
   class="org.springframework.web.servlet.view.json.MappingJacksonJsonView">
        <property name="prefixJson" value="true" />
      </bean>
      <bean
   class="org.springframework.web.servlet.view.xml.MarshallingView">
        <property name="marshaller">
          <bean class="org.springframework.oxm.castor.CastorMarshaller" />
        </property>
      </bean>
    </list>
  </property>
  <property name="viewResolvers">
    <list>
      <bean class="org.springframework.web.servlet.view.BeanNameViewResolver" />
      <bean class="org.springframework.web.servlet.view.UrlBasedViewResolver">
        <property name="viewClass"  value="org.springframework.web.servlet.view.JstlView" />
        <property name="prefix" value="/view/" />
        <property name="suffix" value=".jsp" />
      </bean>
    </list>
  </property>
</bean>
```
  
  

## 3. 테스트
`ContentNegotiatingViewResolver`를 적용하므로 @Controller에서는 어떠한 뷰를 리턴하도록 작성해도 전혀 상관이 없다. curl로 테스트한 결과는 아래와 같다.  
  
결과) curl http://xxx/person/2 (xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<employee-vO age="10" man="true" id="2"><name>aaa</name></employee-vO>
```
  
