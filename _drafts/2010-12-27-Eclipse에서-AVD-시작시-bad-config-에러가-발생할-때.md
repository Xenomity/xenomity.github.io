---
title: "Eclipse에서 AVD 시작시, bad config 에러가 발생할 때"
tags: [android, eclipse]
date: 2010-12-27T15:51:48+09:00
---

[##\_1C|cfile27.uf.2457333E5301923908017A.jpg|width="447" height="298" filename="201011231303.jpg" filemime="image/jpeg"|\_##]

  
android avd 관련 파일이 인식할 수 없는 문자셋이 포함된 경로에 위치할 경우,  
'ERROR: bad config: virtual device directory lacks config.ini' 에러가 발생한다.  

이 경우에 avd의 경로를 다른 경로로 옮겨주면 해결된다.  

[##\_1C|cfile7.uf.261FE5445301924604E837.jpg|width="437" height="98" filename="201011231309.jpg" filemime="image/jpeg"|\_##]

cmd \> android list avd
  

[##\_1C|cfile9.uf.265C20455301925820416F.jpg|width="590" height="32" filename="201011231313.jpg" filemime="image/jpeg"|\_##]

cmd \> android move avd -n {이동시킬 avd 명} -p {이동시킬 경로}
  
예) android move -n Android\_2.2\_Emulator -p c:\avd2.2  
