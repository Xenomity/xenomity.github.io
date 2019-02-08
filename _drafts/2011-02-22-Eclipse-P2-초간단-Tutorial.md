---
title: "Eclipse P2 초간단 Tutorial"
tags: [eclipse, osgi, p2, rcp]
date: 2011-02-22T15:14:00+09:00
---

<font color="#d18e0a">* Eclipse 3.6 Helios 기반으로 작성함.<br>
</font>  
Eclipse의 업데이트 방식이 P2로 바뀌면서 기존 org.eclipse.update.ui 클래스들은 전부 Deprecated되었다.  
  
![Deprecated Eclipse Update API](/assets/image/2011-02-22-201102221408.jpg)
  
그래서 기존 Product를 수정하면서 P2 예제를 간단하게 만들어 보았다.  
  
  

#### **1. 업데이트 가능한 Product 만들기**
1) Hello RCP Template을 통해 test.p2 Project (Plug-In Project) 생성.  
- `plugin.xml`

![plugin.xml](/assets/image/2011-02-22-201102221415.jpg)

2) test.p2 Project에 test.p2.product (Product Configuration) 생성.  
- Feature 기반 구성.  

![product](/assets/image/2011-02-22-201102221419.jpg)

3) test.p2.feature (Feature Project) 생성.  
- Plug-In 탭 -> test.p2 Plug-In을 추가한다.

![feature.xml](/assets/image/2011-02-22-201102221431.jpg)

4) test.p2.feature Project에 category.xml (Category Definition) 생성.
- 원하는 카테고리명과 해당 카테고리에 포함할 Feature를 추가한다.  

![category.xml](/assets/image/2011-02-22-201102221437.jpg)

5) Feature 내보내기.  
- test.p2.feature Project -> feature.xml -> Exporting -> Export Wizard  

![export feature 1](/assets/image/2011-02-22-201102221446.jpg)

![export feature 2](/assets/image/2011-02-22-201102221447.jpg)
Category repository : test.p2.feature Project에 생성된 category.xml 파일의 절대경로  
  
6) 테스트.  
Feature 내보내기가 완료되었으면, Eclipse IDE의 Help -> Install New Software에서 정상적으로 나타나는지 확인한다.  

![export feature 2](/assets/image/2011-02-22-201102221455.jpg)
Work with : Update Site Address  
  
  

#### **2. RCP에 직접 P2 UI 추가**
RCP에서 직접 P2 UI를 제공하기 위해서는 org.eclipse.equinox.p2.user.ui Feature가 필요하다.

![product](/assets/image/2011-02-22-201102221423.jpg)
- test.p2.product -  


P2 제공 Command ID  
```
org.eclipse.equinox.p2.ui.sdk.update  (Check for updates)  
org.eclipse.equinox.p2.ui.sdk.install  (Install New Software...)  
```
  
예) 메뉴 혹은 액션으로 할당시, 위 Command ID를 통해 처리를 위임시킨다.  

![handle p2 command](/assets/image/2011-02-22-201102221510.jpg)

(RCP에서 P2를 테스트하기 위해서는 꼭 Export한 후 테스트한다. Eclipse에서 실행하는 경우 'Cannot complete the request' 예외가 발생한다.)
