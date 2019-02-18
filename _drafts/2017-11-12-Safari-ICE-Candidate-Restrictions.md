---
title: "Safari ICE Candidate Restrictions"
tags: [webrtc, ice]
date: 2017-11-12 03:00:00
---

## Problem
Safari v11 이후부터 미디어 캡쳐 장치에 대한 접근 권한이 최종 사용자에 의해 확보되지 않으면, 더 이상 Host ICE 후보 주소들의 노출을 허용하지 않는다. 실제로 테스트를 해 보면, UDP Candidate Type이 `host`를 제외한 나머지 형식의 후보들만 노출되는 것을 확인할 수 있다. 이와 관련한 내용은 다음 링크에 자세히 설명되어 있다.
- [https://webkit.org/blog/7763/a-closer-look-into-webrtc/](https://webkit.org/blog/7763/a-closer-look-into-webrtc/)

요약하자면, 최신 Webkit에서 Host ICE 후보군 주소를 노출하지 않는 이유는 tracing 등과 같은 보안상의 이유와, 실제 피어 간 연결은 서버 재귀 주소나 TURN relay 주소만으로 충분하다는 내용이다. 단, 미디어 캡쳐 장치에 대한 접근 요청을 사용자가 수락하면, 해당 웹 사이트에 대한 높은 수준의 신뢰도를 부여하고 예외로 취급하며 Host ICE 후보를 노출한다. 반대로 보자면, 미디어 캡쳐 장비의 접근이 필요하지 않은 `recvonly` 사용자의 경우에는 호스트 연결이 불가능하다는 얘기가 된다.


## Solution
위 문제를 우회하기 위하여 개발자 옵션을 변경하거나 강제로 갭쳐 장치의 접근 권한을 확보하는 방법을 고려해 볼 수 있다. 전자의 경우 최종 사용자에게 충분한 사전 가이드가 필요할 수 있으며, 후자의 경우 programmatically 사용자 동의 수준에서 직접적인 호스트 연결이 가능하다. 직접적인 Host IP 노출이 민감하지 않은 사내 전용 서비스 등에 적합한 방법이다.

### 개발자 옵션 활성화
- Preferences -> 개발자 메뉴 활성화
- Menu -> Develop -> WebRTC -> Disable ICE Candidate Restriction

### 강제로 캡쳐 장치 접근 권한을 확보 (trick code)
```javascript
if (isSafari) {
    await navigator.mediaDevices.getUserMedia({ audio: true, video: false });
}
```

## Conclusion
시청자의 입장에서 미디어 캡쳐 장치에 대한 접근 수락을 해야 하는 흐름이 비-상식적이나, 접근성이 떨어지는 개발자 옵션 활성화 방법보다 나은 경우도 있다. 권장하는 방법은 아니며, 정공법으로 해결할 수 있는 인프라 구축이 필요하다.


## References
- Safari 11 Developer Previews: https://webkit.org/blog/7763/a-closer-look-into-webrtc/
- ICE: https://blog.xenomity.com/ICE_Interactive_Connectivity_Protocol
