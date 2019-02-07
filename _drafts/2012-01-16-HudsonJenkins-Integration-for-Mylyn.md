---
title: "Hudson(Jenkins) Integration for Mylyn"
tags: [eclipse, hudson, jenkins, mylyn]
date: 2012-01-16T19:30:39+09:00
---

Mylyn은 Trac/Jira와 같은 Issue Tracker나 SCM, CI, 기타 build 도구들과의 통합 및 모니터링을 지원하여 좀 더 빠른 작업 및 모니터링, 문서화가 가능하게 한다. 또한 Outlook, Gmail 등의 연동으로 일정, 작업리스트, 메일 등의 통합관리도 지원하는 개인적으로 좋아하는 플러그인 중의 하나이다.. ^\_^;  
  
이번에는 대표적인 CI 도구인 Hudson을 Mylyn과 통합하는 방법을 포스팅해 본다. Mylyn에서는 Hudson connector를 통한 Build History, 각 빌드별 Output Console, Job Execution, JUnit 테스트 결과 등의 통합환경을 제공한다.  
  
<font color="#d18e0a">* 작업환경 : Eclipse 3.7 Indigo, Hudson 2.2.0</font>

#### **1. Install Hudson connector for Mylyn**
Task List View -\> New Task -\> Add Repository  

[##\_1C|cfile5.uf.226E4B3E5301995827AA6F.jpg|width="225" height="264" filename="201201161000.jpg" filemime="image/jpeg"|\_##]

  
Click Install More Connectors...  

[##\_1C|cfile25.uf.275F8A3E53019963353007.jpg|width="556" height="434" filename="201201161741.jpg" filemime="image/jpeg"|\_##]

  
Build Management -\> Hudson/Jenkins   

[##\_1C|cfile10.uf.276BD043530199710D5581.jpg|width="578" height="723" filename="201201161743.jpg" filemime="image/jpeg"|\_##]

  
Select Mylyn Builds Connector: Hudson/Jenkins (Incubation)  

[##\_1C|cfile30.uf.2524A2465301997D1E551C.jpg|width="590" height="403" filename="201201161744.jpg" filemime="image/jpeg"|\_##]

  
  

#### **2. Build Server 등록**
Mylyn Build View -\> New Build Server Location -\> Add Build Server  

[##\_1C|cfile25.uf.213C0F3D530199880B4932.jpg|width="223" height="166" filename="201201161111.jpg" filemime="image/jpeg"|\_##]

  
Select Hudson  

[##\_1C|cfile22.uf.232895435301999331072B.jpg|width="525" height="500" filename="201201161921.jpg" filemime="image/jpeg"|\_##]

  
Server : Hudson URL  
Label : Mylyn에서 보여질 Task명  
User/Password : 익명(Anonymous) 로그인이 아니라면, Hudson 계정 입력  
  
이 과정을 거치고 Validate 버튼을 클릭하면, Hudson에 등록된 Job 목록이 Build Plans 리스트에 보여진다.  
Mylyn과 연동을 원하는 Job을 선택한다.  

[##\_1C|cfile3.uf.21297A405301999F28AA07.jpg|width="525" height="558" filename="201201161922.jpg" filemime="image/jpeg"|\_##]

  
정상적으로 연동되면, Eclipse Notofication 및 Job 목록을 확인할 수 있다.  

[##\_1C|cfile4.uf.254A3940530199B116A44D.jpg|width="205" height="151" filename="201201161923.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile5.uf.25606643530199BD16A051.jpg|width="224" height="160" filename="201201161924.jpg" filemime="image/jpeg"|\_##]

  

[##\_1C|cfile6.uf.273C1A42530199CE2CCD13.jpg|width="590" height="498" filename="201201161925.jpg" filemime="image/jpeg"|\_##]

