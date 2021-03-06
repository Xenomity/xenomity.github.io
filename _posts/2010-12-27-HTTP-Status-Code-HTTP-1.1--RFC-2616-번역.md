---
title: "HTTP Status Code (HTTP 1.1 : RFC 2616) 번역"
tags: http
date: 2010-12-27T15:59:31+09:00
---

### HTTP Status Code(HTTP 1.1 : RFC 2616)
  
상태코드는 서버가 요구 메시지를 수신하여 처리한 결과를 알려주는 세 자리의 정수로 된 처리 결과 번호입니다.  
첫 번째 자리 숫자는 응답의 종류에 대한 분류 기호이며, 나머지 두 자리 숫자는 일련번호입니다. 현재 첫 번째 자리 숫자에 대해 다섯 가지로 분류하여 쓰고 있습니다.  
  

- Informatinal 1xx 
- Success 2xx 
- Redirection 3xx 
- Client Error 4xx 
- Server Error 5xx 

* * *
  

#### Informational 1xx
참고 정보로 클라이언트의 요청이 접수되었고 현재 처리하고 있다는 의미입니다.  
클라이언트에서 첨부문서(attatched document)를 보내기 전에 요청을 보낼때 Expect헤더에 설정해서 보냅니다. 잠적적인 응답을 표시하며 Status-Line과 선택적인 헤더로 구성되어 있습니다. 이 클래스는 빈 라인으로 종료되고 HTTP/1.0은 어떠한 1xx 상태 코드로 정의하지 않기 때문에 실험적인 상황 이외에 서버는 1xx 응답을 HTTP/1.0 클라이언트에 발송해서는 안됩니다.   
**100 Continue (계속)**  
요청된 초기 부분은 접수되었고 클라이언트는 계속해서 요청할 수 있다는 것입니다.  
이 잠정적인 응답은 클라이언트에게 응답의 시초 부분이 수신되었으며 서버가 아직 거부하지 않았음을 알리는 데 사용합니다. 클라이언트는 요구의 나머지 부분을 발송하여야 하며 요구가 완료 되었으면 이 응답을 무시해야 합니다. 서버는 요구가 완료된 다음 마지막 응답을 발송합니다.   
**101 Switching Protocols (프로토콜 변환)**  
서버는 Upgrade 헤더 필드에 명시된 프로토콜로 교환하기 위한 클라이언트 요청에 따르고 있다는 것을 말합니다.  
서버가 이해하였으며 기꺼이 Upgrade 메시지 헤더 필드를 통하여 접속에 사용되고 있는 애플리케이션 규약 변경에 관한 클라이언트의 요구에 따릅니다. 서버는 101 응답을 종료하는 빈 라인 바로 다음 응답 메시지의 Upgrade 헤더 필드가 정의한 규약으로 전환할 것입니다.   
  
규약은 전환하는 것이 유리한 경우에만 전환됩니다. 예를 들어 새로운 버전의 HTTP로 전환하는 것이 이전 버전을 사용하는 것보다 유리하며 해당 기능을 사용하는 자원을 배달할 때 실시간, 동시 규약으로 전환하는 것이 유리합니다.   
  

#### Success 2xx
요청 받은 것이 성공적으로 처리되었음을 나타냅니다.  
이 상태 코드 클래스는 클라이언트의 요구가 성공적으로 수신, 해석 및 접수되었음을 표시합니다.   
**200 OK**  
클라이언트의 요청이 성공적이었으며, 서버는 요청한 데이터를 포함하여 응답합니다.  
응답과 함께 리턴 되는 정보는 요구에 사용된 method에 달려 있습니다.   

예를 들면: GET 요구한 자원에 상응하는 엔터티는 응답에 포함되어 발송됩니다. HEAD 요구한 자원에 상응하는 Entity-Header 필드는 Message-Body 없이 응답에 포함되어 발송됩니다. POST 처리 결과를 설명 또는 포함하는 엔터티. TRACE 수신 서버가 수신한 요구 메시지를 포함하고 있는 엔터티.   
**201 Created (생성 되었음)**  
새로운 URI가 만들어질 때마다 사용되며 결과 코드와 함께 새로운 데이터가 위치한 곳을 지정하기 위해 Location 헤더가 서버에 의해 주어집니다.  
원서버는 201 상태 코드를 리턴하기 전에 반드시 자원을 생성해야 합니다. 처리가 즉각적으로 수행될 수 없을 때에 서버는 202(Accepted) 응답으로 대신 응해야 합니다.   
**202 Accepted (접수 되었음)**  
클라이언트이 요청을 받아들이기만 했을 뿐 아직 완료되지 않은 상태를 나타냅니다.  
처리를 위해 응답을 접수하였으나 처리는 완료되지 않았다는 의미로 요구는 엔터티의 처리 과정에서 허용되지 않을 수도 있기 때문에 궁극적으로 처리될 수도 있고 처리되지 않을 수도 있습니다. 이와 같은 동시 작업에서 상태 코드를 재발송하는 설비는 없습니다.   
  
202 응답은 의도적으로 작업을 수행하지 않습니다. 이 응답의 목적은 서버가 사용자 에이전트가 프로세스가 완료될 때까지 서버에 지속적으로 연결되지 않고도 다른 프로세스에 대한 요구(하루에 한 번만 실행되는 배치 지향적인 프로세스일 수도 있습니.)를 접수할 수 있도록 하는 데 있습니다. 이 응답을 리턴하는 엔터티는 상태 점검자(monitor)에 대한 지시자 또는 사용자가 언제 요구가 완료될 수 있는지에 대한 예상 및 요구의 현재 상태에 대한 표시를 포함해야 합니다.   
**203 Non-Authoritative Information(비 인증 정보)**  
Entity-Header의 리턴 된 메타 정보는 서버에서 사용할 수 있는 정의 세트가 아니고 지역 또는 제 3 자의 복사본에서 수집한 것입니다.  
제시된 세트는 원래 버전의 하부 세트 또는 상위 세트일 수 있습니다. 예를 들어 자원에 대한 지역적 주해 정보를 포함하면 원서버가 알고 있는 메타 정보에 대한 상위 세트를 만들어 낼 수도 있습니다. 이 응답 코드를 사용하는 것은 의무사항이 아니며 응답이203이 아니면 200 (OK)일때만 적합합니다.   
**204 No Content(내용이 없음)**  
응답할때 주어지는 헤더이나 응답된 실제 내용이 없다는 뜻입니다.  
새로운 문서가 없어서 브라우저에게 이전 문서를 계속 표시하라고 알려주는 것으로 서버가 요구를 완전히 처리 했으나 반송할 새로운 정보가 없다는 것으로 클라이언트가 사용자 에이전트이면 요구를 발송하도록 한 문서 내용을 변경해서는 안 됩니다.   
  
이런 응답을 받는 이유는 웹브라우저가 문서를 보기위해 갱신을 하지 않았기 때문입니다. 이미지맵에서 클라이언트가 이미지의 영역중 사용하지 않거나 공백인 부분을 클릭했을 때를 처리할 때 유용합니다.   
**205 Reset Content(내용을 지움)**  
새로운 문서가 없더라도 브라우저에서 창을 초기화하고, 문서를 새로 표시한다는 것입니다.  
서버가 요구를 완전히 처리하였으며 사용자 에이전트는 요구를 발송하도록 한 문서의 내용을 지워야합니다. 이 응답은 주로 사용자 입력을 통하여 처리를 위한 입력이 발생하도록 하기 위해 사용합니다. 이 응답 뒤에 입력을 수행한 폼을 지워 사용자가 다른 입력 요구를 쉽게 시작할 수 있게 합니다. 이 응답은 엔터티를 포함해서는 안 됩니다.   
  
웹브라우저가 추가적인 입력을 위해 사용된 트랜잭션을 지우는 것으로 CGI 애플리케이션에서 데이터를 입력받을때 적합합니다.   
**206 Partial Content(부분적 내용)**  
서버가 요청된 크기의 데이터를 반환하고 있다는 것입니다.  
Range 헤더 지정 요청에 응답하는데 이용됩니다. 이 요구는 반드시 원하는 영역을 표시하는 Range 헤더 필드를 포함해야 합니다. 응답은 이 응답에 포함된 영역을 표시하는 Content-Range 헤더 필드나 각 파트의 Content-Range 필드를 포함하는 multipart/byteranges Content-Type을 포함해야 합니다. multipart/byteranges를 사용하지 않았으면 응답의Content-Length 헤더 필드는 Message-Body로 전송된 OCTET의 실제 숫자와 정확하게 일치해야 합니다.   
  
Range 및 Content-Range 헤더를 지원하지 않는 캐시는 206(Partial Content) 응답을 캐시해서는 안됩니다.   

* * *
  

#### Redirection 3xx
파일들이 이동되었을 때 쓰이며, 이동하는 위치를 나타내는 Location 헤더가 응답에 포함됩니다.  
이 상태 코드 클래스는 사용자 에이전트가 요구를 완전히 처리하기 위해서는 추가적인 처리가 필요하다는 것을 표시합니다. 요구되는 처리는 두 번째 요구에 사용된 method가 GET 또는 HEAD일 경우에만 사용자와의 상호작용 없이도 수행될 수 있습니다. 사용자 에이전트는 이러한 방향 재설정이 무한 루프를 표시하는 것이기 때문에 다섯 번 이상 자동적으로 요구 방향 재설정을 해서는 안 됩니다.   
**300 Multiple Choices (복수 선택)**  
요청된 문서가 여러곳에 있을때 어떤 문서를 원하는지를 묻는 것입니다.  
요구된 자원이 각자 자신 특유의 위치를 가지고 있는 표현 세트 중의 하나와 대응되며 사용자(또는 사용자 에이전트)가 선호하는 표현 방식을 선택하고 요구를 해당 위치로 재설정할 수 있도록 에이전트가 주도하는(agent-driven) 협상 정보가 제공됩니다.   
  
HEAD 요구가 아닌 이상 응답은 사용자 또는 사용자 에이전트가 가장 적합한 것을 선택할 수 있는 자원 특징 및 위의 목록을 포함한 엔터티를 포함합니다. 엔터티 포맷은 Content-Type 헤더 필드가 설정한 media type에 의해 명시됩니다. 사용자 에이전트의 포맷 및 성능에 따라 가장 적합한 선택을 결정하는 것은 자동으로 수행될 수 있습니다. 그러나 이 규격은 이러한 자동 선택의 표준에 대하여 아무런 규정도 하지 않습니다.   
  
서버가 선호하는 표시 방법을 가지고 있으면 Location 필드에 해당 표시 방법에 대한 상세한 URL을 포함해야 합니다. 사용자 에이전트는 Location 필드 값을 이용하여 자동으로 방향을 재설정할 수 있습니다. 이 응답은 별도의 표시가 없는 한 캐시할 수 있습니다.   
**301 Moved Permanently (영구 이동)**  
요청된 문서의 위치가 영구적으로 변했음을 나타내는 것입니다.  
요구된 자원에 새로운 영구 URI가 할당되었으며 향후 이 자원에 대한 참조는 리턴 된 URI 중 하나를 이용하여 이루어질 수 있습니다. 링크를 편집할 수 있는 능력이 있는 클라이언트는 가능하다면 Request-URI 에 대한 참조를 서버가 리턴한 하나 또는 그 이상의 새로운 참고처로 자동적으로 재링크시켜야 합니다. 다르게 표시되어 있지 않으면 이 응답은 캐시할 수 있습니다.   
  
이 새로운 URL은 응답 메시지의 Location 필드로부터 전달된 것이어야 하는데, HEAD 요구의 경우가 아니라면 응답의 Entity-Body는 새로운 URL에 대한 하이퍼링크를 가진 짤막한 설명문을 갖고 있어야 합니다.   
  
만약 POST 요구에 대한 응답으로 301 상태코드가 수신되면 사용자 에이전트는 사용자로부터 확인을 받지 않은 상태에서 요구 메시지를 자동 방향전환 시켜서는 안 됩니다. 왜냐하면 이것이 요구 메시지를 발생시킨 상황 조건에 대한 변화를 줄 수 있기 때문입니다.   
  
[주] 301 상태코드를 수신한 후에 POST 요구를 자동 방향전환시키면 현재의 어떤 사용자 에이전트는 GET 요구로 바꾸어버리는 오류상황을 만들기도 합니다.   
**302 Found**  
요청된 URI는 일시적으로 새로운 URI를 가집니다.  
Location 헤더는 새로운 장소를 가리킨다. 만일 이것이 GET 이나 HEAD 메소드에 대한 응답이라면 클라이이언트는 응답을 받자마자 요청을 해결하기 위해 새로운 URI를 사용해야 합니다.   
**303 See Other(다른 것을 참조)**  
요구된 자원이 별도의 URI(Location 헤더에 명시한)에 임시로 보관되어 있으며 해당 자원에서 GET method를 사용하여 조회해야 합니다.  
이 method는 주로 POST가 활성화한 스크립트의 산출물을 사용자 에이전트가 선택된 자원으로 방향을 재설정할 수 있도록 하기 위해 사용됩니다. 새로운 URI는 처음 요구된 자원에 대한 대체 참고처가 아닙니다. 303 응답은 캐시할 수 없으나 두 번째(재설정된) 요구에 대한 응답은 캐시할 수 있습니다.   
  
GET 또는 HEAD 이외의 요구에 대한 응답에 301 상태 코드가 접수되면 사용자 에이전트는 사용자가 확인하지 않는 한 요구를 발행한 조건을 변경할 수도 있기 때문에 자동적으로 요구의 방향을 재설정해서는 안 됩니다.   
**304 Not Modified(변경되지 않았음)**  
브라우저의 캐시에 들어있는 문서가 최신 문서이니 그것을 그대로 사용하라는것을 나타냅니다.  
클라이언트가 조건적 GET 요구를 실행했고 접근할 수 있으나 문서가 변경되지 않았으면 서버는 이 상태코드로 응답해야 합니다. 이 응답은 Message-Body를 포함해서는 안 됩니다.   
  
응답은 다음의 헤더 필드를 포함하고 있어야 합니다.   

? 날짜 ? ETag 및/또는 Content-Location, 동일한 요구에 대한 200 응답 속에 헤더가 발송되었을 경우 ? Expires, Cache-Control, 및/또는 Vary, 동일한 변이에 대한 이전 응답 속에 발송된 field-value가 상이할 경우 조건적 GET이 강한 캐시 검증자를 사용했다면 응답은 다른 Entity-Header를 포함해서는 안 됩니다. 그렇지 않으면(조건적 GET이 약한 캐시 검증자를 사용할 때) 응답은 Entity-Header을 포함해서는 안 됩니다. 이렇게 하여 캐시 된 Entity-Body과 갱신된 헤더 사이의 불일치를 방지할 수 있습니다.   
304 응답이 현재 캐시 되지 않은 엔터티를 표시할 때 캐시는 이 응답을 무시하고 조건 없이 요구를 반복해야 합니다.   
캐시가 수신한 304 응답을 캐시 엔트리의 갱신에 사용한다면 캐시는 응답이 가지고 있는 새로운 필드 값을 반영하기 위해 엔트리를 반드시 갱신해야 한다.   
304 응답은 Message-Body를 포함해서는 안되므로 항상 헤더 필드 다음의 첫 공백 라인으로 종료되어야 합니다.   
**305 Use Proxy(프락시를 사용할 것)**  
요청된 문서를 프록시를 통해서만 전송 받으라는 것을 나타냅니다.  
요구된 자원을 Location 필드에 명시된 프락시를 통하여 접근해야만 합니다. Location 필드가 프락시의URL을 제공합니다. 수신측은 프락시를 통한 요구를 반복할 것으로 기대됩니다.   
**307 Temporary Redirect(임시 이동)**  
요청된 URI가 일시적으로 옮겨졌다는 뜻입니다.  
Location 헤더가 새로운 장소를 가르킵니다. 이 상태 코드를 받는 즉시, 클라이언트는 요청을 해결하기 위해 새로운 URI를 사용해야 하지만 앞으로 모든 요청들은 이전의 URI를 사용할 것입니다.   

* * *
  

#### Client Error 4xx
클라이언트의 요청이 불안전하며, 클라이언트 요청을 성공시키려면 다른 정보가 필요하다는 것을 말합니다.  
상태 코드의 4xx 클래스는 클라이언트가 에러를 발생한 것처럼 보일 경우에 사용됩니다. HEAD 요구에 응답하는 경우를 제외하고는 서버는 임시적이건 영구적이건 에러 상황에 대한 설명을 포함한 엔터티를 포함해야 합니다. 이러한 상태 코드는 모든 요구 method에 적용할 수 있습니다. 사용자 에이전트는 사용자에게 포함된 엔터티를 표시해야 합니다.   
  
[주] 클라이언트가 데이터를 발송한다면 TCP를 사용하는 서버 구현 방식은 서버가 입력 접속을 종료하기 전에 응답을 포함하고 있는 패킷 접수를 확인할 수 있도록 주의해야 합니다. 클라이언트가 접속이 종료된 후에도 계속해서 데이터를 전송한다면 서버의 TCP 스택은 리셋 패킷을 클라이언트에게 발송할 것입니다.  
이 리셋 패킷은 HTTP 애플리케이션이 읽거나 해석하기 전에 클라이언트가 확인한 입력 버퍼를 지웁니다.   
**400 Bad Request(잘못된 요구)**  
클라이언트의 요청에 문법적인 오류가 있다는 것을 서버가 알아냈다는 것을 의미합니다.  
잘못된 형식 때문에 서버가 요구를 이해할 수 없습니다. 클라이언트는 변경 없이 요구를 반복해서는 안 됩니다.   
**401 Unauthorized (인증되지 않았음)**  
클라이언트가 잘못된 인증정보를 Authorization 헤더에 넣었음을 나타냅니다.  
응답이 사용자 인증을 요구합니다. 이 응답은 요구된 자원에 적용할 수 있는 설명 요구(challenge)를 포함하고 있는 WWW-Authenticate 헤더 필드를 포함하고 있어야 합니다. 클라이언트는 적절한 Authorization 헤더 필드를 가지고 요구를 반복할 수 있습니다. 요구가 벌써 Authorization 증명서를 포함하고 있다면 401 응답은 해당 증명서에 대한 인증이 거절되었음을 표시합니다. 401 응답이 이전 응답과 동일한 설명 요구를 포함하고 있고 사용자 에이전트가 한 번 이상 인증 획득을 시도했다면 해당 엔터티가 관련된 진단 정보를 포함하고 있기 때문에 사용자에게 응답에 표시된 엔터티를 표시해주야 합니다.   
**402 Payment Required**  
이 코드는 아직 HTTP로 구현되지 않았습니다. 하지만 언젠가는 서버의 문서를 받아보기 위해 지불이 필요하다는 것을 나타냅니다.   
**403 Forbidden(금지되었음)**  
클라이언트의 인증정보에 상관없이 페이지에 대한 접근을 거부한다는 것을 나타냅니다.  
서버가 요구를 이해했으나 완료하는 것을 거절하고 있다는 의미로 인증은 적용되지 않으며 요구를 반복될 수 없습니다. 요구 method가 HEAD가 아니고 서버가 왜 요구가 완료되었는지 알리고 싶다면 엔터티 안에 거절한 이유를 기록해야 합니다. 이 상태 코드는 서버가 요구가 거부 사유를 밝히기 원하지 않을 때나 다른 응답을 적용할 수 없을 때 일반적으로 사용됩니다.   
**404 Not Found(찾을 수 없음)**  
클라이언트가 요청한 자원에 서버에 없다는 것을 나타냅니다.  
서버가 Request-URI와 일치하는 것을 아무것도 발견하지 못했다는 의미로 이러한 상태가 잠정적인지 영구적인지 관한 아무런 표시도 주어지지 않은 경우입니다.   
  
서버가 이 정보를 클라이언트에게 알리고 싶지 않을 경우 상태 코드 403(Forbidden)을 대신 사용할 수 있습니다. 내부적으로 환경을 설정할 수 있는 메커니즘을 통하여 이전의 자원을 영구적으로 사용할 수 없으며 전송 주소가 없다는 것을 알 수 있으면 410(Gone) 상태 코드를 사용합니다.   
**405 Method Not Allowed(Method를 사용할 수 없음)**  
Allow 헤더와 함께 클라이언트가 사용한 메소드가 이 URI에 대해 지원되지 않는다는 의미입니다.  
Request-Line에 명시된 method를 Request-URI로 확인할 수 있는 자원에서 사용할 수 없습니다.  
응답은 요구된 자원에 사용할 수 있는 method의 목록을 포함한 Allow 헤더를 포함해야 합니다.   
**406 Not Acceptable(접수할 수 없음)**  
클라이언트가 지정한 URI는 존재하지만 클라이언트가 원하는 형식이 아닙니다.  
코드와 함께 서버는 Content-Language, Content-Encoding, Content-Type 헤더를 제공합니다. HEAD 요구가 아닌 이상 응답은 사용자 또는 사용자 에이전트가 가장 적합한 것을 선택할 수 있는 자원 특징 및 위의 목록을 포함한 엔터티를 포함합니다. 엔터티 포맷은 Content-Type 헤더 필드가 설정한 media type에 의해 명시됩니다. 사용자 에이전트의 포맷 및 성능에 따라 가장 적합한 선택을 결정하는 것은 자동으로 수행될 수 있습니다. 그러나 이 규격은 그러한 자동 선택의 표준에 대하여 아무런 규정도 하지 않습니다.   
  
[주] HTTP/1.1 서버는 요구 메시지와 함께 발송된 Accept 헤더에 의해서 접수할 수 없는 응답을 리턴할 수 있게 합니다. 어떤 경우엔 이것이 406 응답을 발송하는 것보다 좋을 수도 있습니다. 사용자 에이전트는 도착하는 응답의 헤더를 검사하여 그것의 접수 여부를 결정하도록 추천합니다. 응답을 접수할 수 없을 때 사용자 에이전트는 잠정적으로 더 이상의 데이터를 수신하지 말아야 하며 추가 행동을 취할 것인지 사용자에게 질의합니다.   
**407 Proxy Authentication Required(프락시 인증 필요)**  
이 코드는 401(Unauthorized)과 유사하지만 클라이언트는 먼저 프락시에서 자기 자신을 인증해야 한다는 것을 표시 하는 것으로 proxy 서버로 로그온 한 후에 다시 시도해 봐야 합니다.  
프락시는 요구된 자원의 프락시에 적용할 수 있는 설명 요구를 포함하는 Proxy-Authenticate 헤더 필드를 리턴해야 합니다. 클라이언트는 적절한 Proxy-Authorization 헤더 필드와 함께 요구를 반복해야 합니다.   
**408 Request Timeout(요구 시간 초과)**  
클라이언트의 모든 요청이 지정한 시간(일반적으로 서버를 구성할때 명시한다) 동안 처리되지 않았음을 의미합니다.  
서버는 네트워크를 끊습니다. 클라이언트가 서버가 기다리도록 준비한 시간 내에 요구를 만들어 낼 수 없는 경우며 클라이언트는 나중에 변경 없이 요구를 반복할 수 있습니다.   
**409 Conflict(충돌)**  
다른 요청이나 서버의 구성과 충돌이 있음을 나타냅니다.  
충돌에 대한 정보는 응답되는 데이터의 일부로 반환됩니다. 이 코드는 사용자가 충돌을 해결하고 요구를 재전송할 수 있을 것으로 기대할 수 있는 상황에서만 사용할 수 있습니다. 응답 본문은 사용자가 충돌의 원인을 인지할 수 있도록 충분한 정보를 포함해야 합니다. 이상적으로는 응답 엔터티가 사용자 또는 사용자 에이전트가 문제를 해결할 수 있을 정도의 충분한 정보를 포함할 수 있을 것입니다. 그러나 가능하지 않을 수도 있으며 필수 사항은 아닙니다.   
  
충돌은 PUT 요구에 대한 응답으로 발생할 가능성이 높습니다. 버전 관리를 사용하고 있고 PUT 요구를 하는 엔터티가 이전 요구(제 3 자)가 작성한 요구와 충돌되는 자원에 대한 변경 사항을 포함하고 있다면 서버는 409 응답을 사용하여 요구를 완료할 수 없음을 표시해야 합니다. 이 경우 응답 엔터티는 응답 Content-Type이 규정한 형식으로 두 버전 사이의 차이점 목록을 포함해야 합니다.   
**410 Gone (내용물이 사라졌음)**  
요청된 문서가 사라지고, 새로운 주소는 알 수 없다는 것을 나타냅니다.  
요구된 자원이 서버에 더 이상 존재하지 않으며 전송 주소를 알 수 없는 경우입니다. 이 조건은 영구적인 것으로 간주해야 합니다. 링크를 편집할 기능이 있는 클라이언트는 사용자 인증 후의 Request-URI에 대한 참고는 삭제해야 합니다. 서버가 그 조건이 영구적인지 여부를 알 수 없거나 결정할 시설이 없으면 상태 코드 401(Unauthorized)을 대신 사용해야 합니다. 다르게 표시되지 않는 한 이 응답은 캐시할 수 있습니다.   
  
410 응답은 주로 수신측에게 자원을 의도적으로 사용할 수 없게 하였고 서버의 소유주가 해당 자원에 대한 원격 링크를 제거하고자 한다는 것을 알림으로써 웹 유지 작업을 지원하기 위해 사용됩니다. 이러한 일은 제한된 시간, 선전용 서비스 및 서버의 사이트에서 더 이상 일하지 않는 개인에게 소속된 자원에서 공통적으로 발생할 수 있습니다. 영구적으로 사용할 수 없는 모든 자원을 "사라진" 것으로 표시하거나 특정 시간 동안 표시를 유지할 필요는 없습니다.   
**411 Length Required(길이가 필요함)**  
서버가 규정된 Content-Length 없는 요구 접수를 거부하였다는 의미로 요구 메시지 내의 Message-Body의 길이를 포함하는 유효한 Content-Length 헤더 필드를 추가한다면 클라이언트는 요구를 반복할 수 있습니다.   
**412 Precondition Failed(사전 조건 충족 실패)**  
하나 또는 그 이상의 Request-Header에 명시된 조건에 의해 요청을 평가하여 false값을 가지는 경우입니다. 이 응답 코드는 클라이언트가 현재 자원의 메타 정보에 사전 조건을 부여할 수 있게 하여 의도하지 않는 자원에 요구 method를 적용하는 것을 방지합니다.   
**413 Request Entity Too Large(요구 엔터티가 너무 큼)**  
서버는 실제 본문이 너무 커서 요청을 처리할 수 없다는 것을 의미합니다.  
요구 엔터티가 서버가 처리할 수 있거나 처리하려는 것보다 크기 때문에 서버가 요구 처리를 거부하는 경우로 서버는 클라이언트가 계속적으로 요구하는 것을 방지하기 위하여 연결을 종료합니다.   
  
조건이 잠정적이면 서버는 Retry-After 헤더 필드를 포함하여 조건이 잠정적이며 얼마 후에 클라이언트가 재시도할 것인지를 표시합니다.   
**414 Request-URI Too Long(Request -URI가 너무 김)**  
서버는 요청된 URI가 너무 커서 요청을 처리할 수 없다는 것을 의미합니다.  
Request-URI가 서버가 해석할 수 있는 것보다 크기 때문에 서버가 요구 처리를 거부하는 경우로 이처럼 드문 조건은 클라이언트가 부적절하게 질의 정보가 긴 POST 요구를 GET 요구로 변환했을때, 클라이언트가 방향 재설정의 URL "블랙 홀"로 빠졌을 때(방향이 재설정된 URL 접두사가 자신의 접미사를 지칭할 때), Request-URI를 읽거나 조작하는 고정-길이 버퍼를 사용하는 몇몇 서버에 존재하는 보안의 허점을 이용하려는 클라이언트로부터 서버가 공격을 받을 때만 발생하는 것 같습니다.   
**415 Unsupported Media Type(지원되지 않는 media type)**  
서버는 실제 본문이 지원되지 않는 형식이라 처리할 수 없다는 의미입니다.  
요구의 엔터티가 요구 받은 method의 자원이 지원하지 않는 포맷으로 구성되어 있기 때문에 요구처리를 거부하였다는 의미입니다.   
**416 Requested range not satisfiable**  
서버는 어떤 유효한 값도 포함하지 않은 Range 헤더를 찾아냈습니다.  
추가로 If-Range헤더는 없어졌습니다.   
**417 Expectation Failed**  
Exept헤더에서 명시된 조건은 만족될 수 없습니다.  

* * *
  

#### Server Error 5xx
서버가 에러를 발생시켰으며 요구를 처리할 능력이 없음을 인지한 경우를 표시합니다. HEAD 요구에 응답하는 때를 제외하고는 서버는 에러 상황에 대한 설명및 에러가 잠정적인지 영구적인지에 관한 상황 설명을 포함하는 엔터티를 포함해야 합니다. 사용자 에이전트는 포함된 모든 엔터티를 사용자에게 표시하여야 한다. 이러한 응답 코드는 모든 요구 method에 적용할 수 있습니다.   
**500 Internal Server Error(서버 내부 에러)**  
서버의 일부(예를 들어 CGI 프로그램)가 멈추었거나 설정에서 오류(잘못된 결과나 적절하지 않은 헤더를 생성시키는 경우)가 나타났음을 의미합니다.   
  
**501 Not Implemented(구현되지 않았음)**  
클라이언트의 요청된 행위가 서버에서 수행할 수 없음을 의미합니다.  
이것은 서버가 요구 method를 인지할 수 없고 어떠한 자원을 사용해도 지원할 수 없을 때 적절한 응답입니다.   
**502 Bad Gateway(불량 게이트웨이)**  
서버(또는 프락시)가 다른 서버(또는 프록시)로 부터의 응답이 적절하지 않음을 의미합니다.  
게이트웨이나 프락시 역할을 수행하는 서버가 요구를 완료하려는 시도에서 접근한 업스트림(upstream) 서버로부터 유효하지 않은 응답을 수신했을 경우입니다.   
**503 Service Unavailable(서비스를 사용할 수 없음)**  
서비스를 일시적으로 제공할 수 없으나, 앞으로 복구된다는 의미입니다.  
서버가 현재 잠정적인 오버로딩(overloading)이나 서버의 유지 작업 때문에 요구를 처리할 수 없다는 것으로 이것이 잠정적인 상황이며 얼마 후에는 완화될 수 있다는 것입니다. 알 수 있다면 지연 시간 길이를 Retry-After 헤더에 표시할 수 있습니다. 아무런 Retry-After 정보가 없으면 클라이언트는 500 응답을 처리하는 것처럼 응답을 처리해야 합니다.   
  
[주] 503 상태 코드가 있다는 것이 서버가 오버로드 되었을 때 이것을 반드시 사용해야 된다는것을 의미하지 않습니다. 어떤 서버는 단순히 접속을 거부하고자 합니다.   
**504 Gateway Timeout(게이트웨이 시간 초과)**  
게이트웨이나 프락시 역할을 수행하는 서버가 시간 내에 요구를 완료하려는 시도에서 접근한 업스트림(upstream) 서버로부터 응답을 수신하지 못했을 경우입니다.   
  
게이트웨이나 프락시의 시간이 경과했다는 것만 빼고는 408(Request time-out)과 같습니다.   
**505 HTTP Version Not Supported(지원되지 않는 HTTP 버전)**  
서버가 요구 메시지에서 사용된 HTTP 규약 버전을 지원하지 않거나 지원하기를 거부했다는 의미입니다.  
서버는이 에러 메시지 이외에는 클라이언트가 사용하는 동일한 주요 버전을 사용하여 요구를 완료할 의사나 능력이 없음을 표시합니다. 응답은 왜 해당 버전이 지원되지 않으며 서버가 어떤 규약을 지원하는가를 설명하는 엔터티를 포함해야 합니다.  
