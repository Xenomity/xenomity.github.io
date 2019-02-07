---
title: "About Polling, Long Polling, Stream, WebSocket"
date: 2011-05-04 08:26:28
tags: [http, polling, ajax, stream, websocket]
---

# About Polling, Long-Polling, Streaming, WebSocket
초기의 정적인 웹이 시간이 지나면서 다양한 동적 요소와 복잡한 상호 작용이 요구됨에 따라 다양한 기술로의 발전을 이루어냈다. 여기서는 RIA나 X-Internet과 같은 별도 런타임 환경이 아닌, 순수한 웹 환경의 폴링(Polling) 기법과 연관하여 발전된 방법들을 정리한다.

### Polling
클라이언트가 주기적으로 서버로 자원을 요청(Request)하고 응답(Response)받는 방식이다. -*단일 요청/응답의 원칙*-에 따른 기본적인 HTTP 프로토콜의 흐름이라 보통의 웹 서비스로 많이 사용되지만, 요청에 대한 부담이 크거나 근-실시간 메세지 교환이 자주 발생하는 시스템에서는 어울리지 않는다. 초창기의 웹은 정적인 뷰만 제공하면 되는 서버의 역할이 전부였기 때문이다.

### Long-Polling
클라이언트가 서버로 자원을 요청하면 서버는 클라이언트가 요청한 자원(or event)을 획득할 때까지 대기한다. 지정된 만료 시간 이내에 자원 획득에 성공하면 서버는 대기를 풀고 클라이언트로 응답한다. 그리고 응답을 받은 클라이언트는 다시 위 흐름을 반복한다. HTTP의 비-지속적 연결을 지속적인 연결처럼 흉내(*long-term http connections*)내어 실시간 메세지 교환에 근접하도록 하는 방식이다.  
(* Ajax를 사용하므로 'Ajax Push'라고도 하며, -_Comet_-으로 널리 알려져 있다.)  
Long-Polling은 서버의 자원 변경을 감지하기 위한 목적으로 많이 응용해 사용되는데, 반복적/주기적인 폴링 방식으로 구현하는 것에 비하면 트래픽 낭비를 훨씬 줄일 수 있으나 불필요한 연결을 지속적으로 유지함에 따른 리소스 점유가 발생할 수 있으므로 주의해야 한다.

### Streaming
Long-Polling 방식과 흐름은 비슷하지만 서버는 이벤트가 발생할때마다 응답을 완료하지 않고 부분 데이터(*chunk data, stream*)로 클라이언트에게 전송한다. 이렇게 하면 클라이언트는 재 연결을 시도할 필요가 없어지므로 Long-Polling 방식에 비해 좀 더 효율적이다.
![Polling, Long-Polling, Streaming](/assets/image/polling_long-polling_streaming.gif)
그림 1. Polling vs Long-Polling vs Streaming (이미지 출처:  http://blogs.sun.com/theaquarium/entry/slideshow_example_using_comet_dojo) -

### WebSocket
HTML5 명세에 포함된 기술로서 서버와 클라이언트가 TCP 기반 연결을 통해 지속적인 양방향 메세지 교환이 가능하도록 하는 방식이다. 따라서 **실시간성이 요구되는 시스템**(*e.g. web-chat, game, notification-server, etc*)을 손쉽게 구현할 수 있는 토대를 제공한다.  
WebSocket은 위 방식들과는 다르게 프로토콜 수준에서 제공되는 방식이다. 최초 연결 확립은 HTTP로 시도하고, 독자적인 WebSocket 프로토콜(*'ws' uri scheme*)로 연결을 유지한다. WebSocket API는 W3C, Protocol은 IETF가 표준화를 담당하고 있다.

### References
* http://helloworld.naver.com/helloworld/textyle/1052
* http://en.wikipedia.org/wiki/Comet_(programming)
