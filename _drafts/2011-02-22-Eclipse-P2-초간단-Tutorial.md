---
title: "Eclipse P2 초간단 Tutorial"
tags: [eclipse, osgi, p2, rcp]
date: 2011-02-22T15:14:00+09:00
---

<font color="#d18e0a">* Eclipse 3.6 Helios 기반으로 작성함.<br>
</font>  
Eclipse의 업데이트 방식이 P2로 바뀌면서 기존 org.eclipse.update.ui 클래스들은 전부 Deprecated되었다.  
  

[##\_1C|cfile21.uf.24172F36530195E82F9248.jpg|width="499" height="174" filename="201102221408.jpg" filemime="image/jpeg"|\_##]

  
그래서 기존 Product를 수정하면서 P2 예제를 간단하게 만들어 보았다.  
  
  

#### **1. 업데이트 가능한 Product 만들기**
1) Hello RCP Template을 통해 test.p2 Project (Plug-In Project) 생성.  

[##\_1C|cfile30.uf.24781B38530195F7240AB4.jpg|width="409" height="302" filename="201102221415.jpg" filemime="image/jpeg"|\_##]

- test.p2 Project plugin.xml -  
  

2) test.p2 Project에 test.p2.product (Product Configuration) 생성.  
Feature 기반 구성.  

[##\_1C|cfile21.uf.2676703B5301960401EAB6.jpg|width="590" height="310" filename="201102221419.jpg" filemime="image/jpeg"|\_##]

- test.p2.product -  
  

3) test.p2.feature (Feature Project) 생성.  
Plug-In 탭 -\> test.p2 Plug-In을 추가한다.

[##\_1C|cfile21.uf.210D9639530196130BBD2D.jpg|width="397" height="468" filename="201102221431.jpg" filemime="image/jpeg"|\_##]

- feature.xml -  
  

4) test.p2.feature Project에 category.xml (Category Definition) 생성.

원하는 카테고리명과 해당 카테고리에 포함할 Feature를 추가한다.  

[##\_1C|cfile21.uf.2671B53B53019621057770.jpg|width="590" height="241" filename="201102221437.jpg" filemime="image/jpeg"|\_##]

- category.xml -  
  

5) Feature 내보내기.  
test.p2.feature Project -\> feature.xml -\> Exporting -\> Export Wizard  

[##\_1C|cfile27.uf.23121D3A5301963136E6B5.jpg|width="578" height="655" filename="201102221446.jpg" filemime="image/jpeg"|\_##]

Directory : 원하는 경로로...  
  

[##\_1C|cfile1.uf.2377A634530196441BFD81.jpg|width="578" height="655" filename="201102221447.jpg" filemime="image/jpeg"|\_##]

Category repository : test.p2.feature Project에 생성된 category.xml 파일의 절대경로  
  
6) 테스트.  
Feature 내보내기가 완료되었으면, Eclipse IDE의 Help -\> Install New Software에서 정상적으로 나타나는지 확인한다.  

[##\_1C|cfile9.uf.217FAD3C5301965407A3AB.jpg|width="590" height="465" filename="201102221455.jpg" filemime="image/jpeg"|\_##]

Work with : Update Site Address  
  
  

#### **2. RCP에 직접 P2 UI 추가**

RCP에서 직접 P2 UI를 제공하기 위해서는 org.eclipse.equinox.p2.user.ui Feature가 필요하다.

[##\_1C|cfile6.uf.222CB23B5301966432D356.jpg|width="510" height="468" filename="201102221423.jpg" filemime="image/jpeg"|\_##]

  

- test.p2.product -  

  

P2 제공 Command ID  

org.eclipse.equinox.p2.ui.sdk.update  (Check for updates)  
org.eclipse.equinox.p2.ui.sdk.install  (Install New Software...)  

  
예) 메뉴 혹은 액션으로 할당시, 위 Command ID를 통해 처리를 위임시킨다.  

[##\_1C|cfile9.uf.22583F3A530196723E0665.jpg|width="590" height="229" filename="201102221510.jpg" filemime="image/jpeg"|\_##]

<font color="#d18e0a">(RCP에서 P2를 테스트하기 위해서는 꼭 Export한 후 테스트한다. Eclipse에서 실행하는 경우 'Cannot complete the request' 예외가 발생한다.)<br>
</font>

