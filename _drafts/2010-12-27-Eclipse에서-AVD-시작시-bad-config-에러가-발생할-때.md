---
title: "Eclipse에서 AVD 시작시, bad config 에러가 발생할 때"
tags: [android, eclipse]
date: 2010-12-27T15:51:48+09:00
---

![Picture 1](/assets/image/2010-12-27-201011231303.jpg)
  
android avd 관련 파일이 인식할 수 없는 문자셋이 포함된 경로에 위치할 경우,  
`ERROR: bad config: virtual device directory lacks config.ini` 에러가 발생한다.  

이 경우에 avd의 경로를 다른 경로로 옮겨주면 해결된다.  

![Picture 2](/assets/image/2010-12-27-201011231309.jpg)

cmd \> `android list avd`
  

![Picture 3](/assets/image/2010-12-27-201011231313.jpg)

cmd \> android move avd -n {이동시킬 avd 명} -p {이동시킬 경로}
  
예) `android move -n Android\_2.2\_Emulator -p c:\avd2.2`
