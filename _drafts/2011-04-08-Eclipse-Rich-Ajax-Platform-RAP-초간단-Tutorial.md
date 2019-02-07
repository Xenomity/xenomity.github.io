---
title: "Eclipse Rich Ajax Platform (RAP) 초간단 Tutorial"
tags: [eclipse, osgi, rap]
date: 2011-04-08T13:07:16+09:00
---

<font color="#d18e0a">* Eclipse 3.6, Rich Ajax Platform (RAP) 1.4 기준으로 작성함.</font>  
  
  
RAP는 Eclipse API를 이용하여 Browser 기반의 Rich Ajax Application을 개발할 수 있도록 한다.  
RCP로 개발된 코드의 거의 80% 이상을 재사용하여 RAP로의 이전이 가능하므로, 기존에 RCP를 개발해 본 사람이라면 거의 동일한 방법으로 쉽게 개발할 수 있다.  
  

|   **RCP** |   **RAP** |
|  OSGi |  OSGi |
|  SWT  - Standard Widget Toolkit |  RWT  - RAP Widget Toolkit |
|  JFace |  JFace |
|  Workbench |  Web-Workbench |

  
  

#### **1. RAP Target Platform 설정**
RAP Runtime을 위한 플러그인 세트를 구성하기 위해 RAP를 임의의 경로로 다운로드한다. 본인의 경우 1.4 릴리즈를 이용했다.  
  
RAP 다운로드 : [http://www.eclipse.org/rap/downloads/](http://www.eclipse.org/rap/downloads/)  
  
Window -\> Preferences -\> Plug-in Development -\> Target Platform -\> Add -\> Nothing: start with an empty target definition  
  
임의의 Target Platform명을 입력하고 Add -\> Directory -\> 위에서 다운로드한 RAP의 압축을 푼 경로/eclipse 선택.  

[##\_1C|cfile21.uf.23396336530197341B9EA3.jpg|width="590" height="541" filename="201104081142.jpg" filemime="image/jpeg"|\_##]

  
생성한 Target Platform을 선택한 후, Apply.

[##\_1C|cfile28.uf.224D1F35530197420FB1DD.jpg|width="525" height="256" filename="201104081219.jpg" filemime="image/jpeg"|\_##]

- 정상적으로 Target Platform이 활성화된 경우 -  
  

#### **2. Hello RAP Template 생성**
RAP의 기본 Template으로 제공되는 Hello RAP를 생성해보면,  
  
New -\> Plug-in Project  

[##\_1C|cfile10.uf.275BE1335301974F31D9C2.jpg|width="525" height="642" filename="201104081229.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile26.uf.2758D5355301975C0B9B67.jpg|width="525" height="642" filename="201104081230.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile28.uf.254A8036530197690C72B9.jpg|width="525" height="642" filename="201104081231.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile25.uf.250B7A395301977A177331.jpg|width="231" height="163" filename="201104081233.jpg" filemime="image/jpeg"|\_##]

- 정상적으로 hello.rap 프로젝트가 생성된 화면 -  
  
  

#### **3. RAP Application 실행**
RAP는 별도의 설정을 하지 않은 경우, 기본적으로 번들되어있는 Jetty Servlet/JSP Container에 의해 구동된다. 53547번의 포트를 점유하게 되며, -Dorg.osgi.service.http.port VM 인자로 원하는 서비스 포트를 변경할 수도 있다.  
  

[##\_1C|cfile2.uf.227485375301978A3E8D8A.jpg|width="271" height="58" filename="201104081249.jpg" filemime="image/jpeg"|\_##]

  
  

[##\_1C|cfile8.uf.267E7639530197991FAC54.jpg|width="590" height="329" filename="201104081238.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile6.uf.226C8A33530197A415F3B8.jpg|width="398" height="437" filename="201104081241.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile4.uf.22189D39530197AE11452F.jpg|width="433" height="144" filename="201104081244.jpg" filemime="image/jpeg"|\_##]

  
  

#### **4. RCP와 RAP의 몇가지 차이점**
1) Base  

|   **RCP** |   **RAP** |
|  OSGi |  OSGi |
|  SWT - Standard Widget Toolkit |  RWT - RAP Widget Toolkit |
|  JFace |  JFace |
|  Workbench |  Web-Workbench |

  
2) MANIFEST.MF의 **require-bundle**.  

|   **RCP** |   **RAP** |
|  org.eclipse.ui |  org.eclipse.rap.ui |

org.eclipse.rap.ui는 RCP와 동일한 코드로 구축 가능하도록 한 org.eclipse.ui 플러그인의 RAP버전이다.  
  
3) Entry Point  

|   **RCP** |   **RAP** |
|  org.eclipse.core.runtime.applications  
 IApplication |  org.eclipse.rap.ui.entrypoint  
 IEntryPoint |

  
예) RCP/RAP Entry Point  

    // RCP public class Application implements IApplication { ... } // RAP public class Application implements IEntryPoint { ... }

