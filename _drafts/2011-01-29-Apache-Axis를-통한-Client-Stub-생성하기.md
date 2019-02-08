---
title: "Apache Axis를 통한 Client Stub 생성하기"
tags: [axis, soap, web-service]
date: 2011-01-29T16:11:00+09:00
---

SOAP 방식의 WebService를 이용하기 위한 Client Stub 및 서비스, 필요 클래스들은 Axis2의 배치 파일을 이용하게 쉽게 생성하고 바인딩할 수 있다.  
  
Axis2를 이용한다면 기본적으로 Stub 파일과 CallbackHandler 파일이 생성되며, 데이터 바인딩에 필요한 필수 클래스들은 전부 Stub 클래스의 Inner 클래스로 만들어지고, Axis를 사용한다면 기본적으로 전부 각기 클래스 파일로 분리되어 생성된다.  
  
Axis2 예) AXIS2\_HOME/bin/wsdl2java.bat
```
wsdl2java.bat -o {Output Path} -uri {WSDL Address}  
```
  
실제 Output Path를 확인해보면, 해당 패키지와 클래스가 생성된 것을 확인할 수 있다.  

![axis-stub](/assets/image/2011-01-29-201103091626.jpg)
  
