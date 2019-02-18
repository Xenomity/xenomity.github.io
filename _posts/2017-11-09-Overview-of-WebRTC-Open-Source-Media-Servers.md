---
title: "Overview of WebRTC Open Source Media Servers"
tags: [webrtc, sfu, mcu]
date: 2017-11-09 01:00:00
---

WebRTC 미들웨어로 검토할만한 오픈 소스 미디어 서버들에 대하여 간략하게 정리하였다. SFU와 MCU에 대한 내용은 [이전 블로그](https://blog.xenomity.com/P2P-vs-SFU-vs-MCU) 참고.

(* 비교 컬럼 기준은 본인이 필요한 feature로 분류.)

OSS	| License | SFU | MCU | Multistream | Video Codec | Datachannel | Simulcast | Opus DTX | VP9 SVC | Trickle ICE | IPv6
----|---------|:---:|:---:|:-----------:|-------------|:-----------:|:---------:|:-------:|:-------:|:-----------:|:----:			
Janus | GPLv3 | O | audio mixing only | X | VP8, VP9, H264 | O | O | X | O | O | O
Kurento | Apache2 | O | O | X | VP8 | O | O | O | X | X | X
mediasoup | ISC | O | X | O | VP8, VP9, H264, H265 | X | O | O | X | X | O
Licode | MIT | O | O | X | VP8, VP9, H264 | X | O | O | O | X | X
Medooze | MIT | O | X | O | VP9 | X | X | X | O | X | X
Jitsi | Apache2 | O | audio mixing only | O | VP8, VP9, H264 | O | O | X | X | O | X

## Janus
- Official Site: https://janus.conf.meetecho.com/
- Server
  - C.
  - Interface
    - HTTP, WebSocket, Unix Domain Socket.
- Client SDK
  - JavaScript
- MCU/SFU
- GPL v3

## Kurento
- Official Site: https://www.kurento.org/
- Server
  - C++.
  - Interface
    - HTTP, WebSocket.
- Client SDK
  - JavaScript, Java.
- SFU
- Apache v2

## Mediasoup
- Official Site: https://mediasoup.org/
- Server
  - C.
  - Interface
    - Client app dependent.
- Client SDK
  - JavaScript.
- MCU/SFU
- MIT

## Licode
- Official Site: http://lynckia.com/licode
- Server
  - C++.
  - Interface
	- HTTP, RabbitMQ.
- Client SDK
  - JavaScript.
- MCU/SFU
- MIT
	

## Jitsi
- Official Site: https://jitsi.org/
- Server
  - Java.
  - Interface
	- HTTP, WebSocket.
- Client SDK
  - JavaScript.
- MCU/SFU
- Apache v2


## References
- P2P vs SFU vs MCU: https://blog.xenomity.com/P2P-vs-SFU-vs-MCU
