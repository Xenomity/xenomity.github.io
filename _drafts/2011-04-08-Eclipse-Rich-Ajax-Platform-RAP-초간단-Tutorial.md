---
title: "Eclipse Rich Ajax Platform (RAP) 초간단 Tutorial"
tags: [eclipse, osgi, rap]
date: 2011-04-08T13:07:16+09:00
---

* Eclipse 3.6, Rich Ajax Platform (RAP) 1.4 기준으로 작성함.

RAP는 Eclipse API를 이용하여 Browser 기반의 Rich Ajax Application을 개발할 수 있도록 한다.  
RCP로 개발된 코드의 거의 80% 이상을 재사용하여 RAP로의 이전이 가능하므로, 기존에 RCP를 개발해 본 사람이라면 거의 동일한 방법으로 쉽게 개발할 수 있다.  
  

## 1. RAP Target Platform 설정
RAP Runtime을 위한 플러그인 세트를 구성하기 위해 RAP를 임의의 경로로 다운로드한다. 본인의 경우 1.4 릴리즈를 이용했다.  
- RAP 다운로드: [http://www.eclipse.org/rap/downloads/](http://www.eclipse.org/rap/downloads/)  
  
Window -> Preferences -> Plug-in Development -> Target Platform -> Add -> Nothing: start with an empty target definition  
  
임의의 Target Platform명을 입력하고 Add -> Directory -> 위에서 다운로드한 RAP의 압축을 푼 경로 `/eclipse` 선택.  

![step 1](/assets/image/2011-04-08-201104081142.jpg)
  
생성한 Target Platform을 선택한 후, Apply.

![step 2](/assets/image/2011-04-08-201104081219.jpg)
- 정상적으로 Target Platform이 활성화된 경우 -  
  

## 2. Hello RAP Template 생성
RAP의 기본 Template으로 제공되는 Hello RAP를 생성해보면,  
  
New -> Plug-in Project  

![step 1](/assets/image/2011-04-08-201104081229.jpg)
  
![step 1](/assets/image/2011-04-08-201104081230.jpg)
  
![step 1](/assets/image/2011-04-08-201104081231.jpg)
  
![step 1](/assets/image/2011-04-08-201104081233.jpg)
- 정상적으로 hello.rap 프로젝트가 생성된 화면 -  
  
  

## 3. RAP Application 실행
RAP는 별도의 설정을 하지 않은 경우, 기본적으로 번들되어있는 Jetty Servlet/JSP Container에 의해 구동된다. 53547번의 포트를 점유하게 되며, -Dorg.osgi.service.http.port VM 인자로 원하는 서비스 포트를 변경할 수도 있다.  
  
![step 1](/assets/image/2011-04-08-201104081249.jpg)
  
![step 1](/assets/image/2011-04-08-201104081238.jpg)
  
![step 1](/assets/image/2011-04-08-201104081241.jpg)
  
![step 1](/assets/image/2011-04-08-201104081244.jpg)
  
  

## 4. RCP와 RAP의 몇가지 차이점
1) Base  

| **RCP** | **RAP** |
|---------|---------|
| OSGi | OSGi |
| SWT - Standard Widget Toolkit | RWT - RAP Widget Toolkit |
| JFace | JFace |
| Workbench | Web-Workbench |

  
2) MANIFEST.MF의 **require-bundle**.  

| **RCP** | **RAP** |
|---------|---------|
| org.eclipse.ui | org.eclipse.rap.ui |

`org.eclipse.rap.ui`는 RCP와 동일한 코드로 구축 가능하도록 한 `org.eclipse.ui` 플러그인의 RAP버전이다.  
  
3) Entry Point  

| **RCP** | **RAP** |
|---------|---------|
| org.eclipse.core.runtime.applications | IApplication |
| org.eclipse.rap.ui.entrypoint | IEntryPoint |

  
예) RCP/RAP Entry Point  
```java
// RCP
public class Application implements IApplication { ... }

// RAP public class Application implements IEntryPoint { ... }
```
