---
title: "Java Components 요약"
tags: java
published: 2010-12-27T16:13:21+09:00
---

다음은 Java SE 5 and J2EE 1.4 명세에 포함되는 자바 컴포넌트 목록과 내용에 대한 요약이다.

### Java SE

- **JFC(Java Foundation Classes)(Swing)**는 Java 기반 클라이언트 애플리케이션의 GUI 및 그래픽 기능 빌드를 지원하는 Java 클래스 라이브러리 세트이다. 
- **JavaHelp** 는 개발자와 작성자가 온라인 도움말을 애플릿, 컴포넌트, 애플리케이션, 운영 체제 및 장치에 통합하여 웹 기반 온라인 문서를 전달할 수 있도록 하는 플랫폼 독립적이고 확장 가능한 도움말 시스템이다. 
- **JNI(Java Native Interface)**를 사용하면 JVM에서 실행되는 Java 코드를 다른 프로그래밍 언어로 작성된 애플리케이션 및 라이브러리와 통합할 수 있다. 
- **JPDA(Java Platform Debugger Architecture)**는 Java SE를 위한 디버깅 지원 인프라이다. 
- **Java 2D API** 는 이미지 작성 및 알파 채널 이미지에 대한 확장 지원을 제공하는 고급 2D 그래픽 및 이미징을 위한 클래스 세트이자 정확한 색상 공간 정의 및 변환을 제공하는 클래스 세트이며 디스플레이 중심 이미징 연산자 세트이다. 
- **Java Web Start** 를 사용하면 사용자가 설치 프로시저를 수행하지 않고 한 번의 클릭으로 모든 기능을 갖춘 애플리케이션을 다운로드하여 실행할 수 있도록 하여 Java 애플리케이션의 전개를 단순화하는 데 도움을 준다. 
- **Certificate API** 는 주제에 대한 공용 키 맵핑을 안전하게 설정하기 위해 인증 경로("인증 체인"으로도 알려져 있음)를 작성, 빌드 및 검증하는 데 필요한 API 세트를 제공한다. 
- **JDBC(Java Database Connectivity)**는 Java 코드의 표 형식 데이터 소스 대부분에 액세스할 수 있도록 하여 광범위한 SQL 데이터베이스에 대한 DBMS 간 연결과 기타 표 형식 데이터 소스에 대한 액세스를 제공하는 API이다. 
- **JAI(Java Advanced Imaging)**는 개발자가 이미지를 쉽게 조작할 수 있도록 하는 단순한 상위 레벨 프로그래밍 모델을 지원하는 오브젝트 지향 인터페이스 세트를 제공하는 API이다. 
- **JAAS(Java Authentication and Authorization Service)**는 표준 PAM(Pluggable Authentication Module) 프레임워크의 Java 버전을 구현하고 사용자 기반 권한 부여를 지원하여 서비스가 사용자에 대한 액세스 제어를 인증하고 적용할 수 있도록 하는 패키지이다. 
- **JCE(Java Cryptography Extension)**는 암호화, 키 생성 및 계약, MAC(Message Authentication Code) 알고리즘을 위한 구현과 프레임워크를 제공하는 패키지 세트이다. JCE는 대칭, 비대칭, 블록 및 스트림 암호에 대한 암호화 지원을 제공하고 보안 설정된 스트림과 봉인된 오브젝트를 지원한다. 
- **JDO(Java Data Object)**는 프로그래머가 Java 도메인 모델 인스턴스를 지속적 저장소(데이터베이스)에 직접 저장하여 잠재적으로 해당 메소드를 직접 파일 I/O, 직렬화, JDBC 및 EJB BMP(Bean Managed Persistence) 또는 CMP(Container Managed Persistence) 엔티티 Bean으로 대체할 수 있도록 하는 지속성의 표준 인터페이스 기반 Java 모델 추상화이다. 
- **JMX(Java Management Extensions)**는 장치, 애플리케이션 및 서비스 구동 네트워크의 관리 및 모니터링에 필요한 웹 기반 모듈 방식 동적 분산 애플리케이션을 빌드하기 위한 도구를 제공한다. 
- **JMF(Java Media Framework)**를 사용하면 Java 애플리케이션 및 애플릿에 오디오, 비디오 및 기타 시간 기반 매체를 추가할 수 있다. 
- **JNDI(Java Naming and Directory Interface)**는 Java 애플리케이션에 엔터프라이즈의 여러 이름 지정 및 디렉토리 서비스에 대한 통합 인터페이스를 제공하여 이기종 엔터프라이즈 이름 지정 및 디렉토리 서비스에 원활하게 연결할 수 있도록 한다. 
- **JSSE(Java Secure Socket Extension)**는 보안 인터넷 통신을 활성화하여 SSL(Secure Sockets Layer) 및 TLS(Transport Layer Security) 프로토콜의 Java 버전을 구현하고 데이터 암호화, 서버 인증, 메시지 무결성 및 선택적 클라이언트 인증을 위한 기능을 포함하는 패키지 세트이다. 
- **JSAPI(Java Speech API)**는 JSGF(Java Speech Grammar Format) 및 JSML(Java Speech Markup Language) 스펙을 포함하며 Java 애플리케이션이 음성 기술을 사용자 인터페이스에 통합할 수 있도록 한다. JSAPI는 크로스 플랫폼 API를 정의하여 명령을 지원하고 인식기, 받아쓰기 시스템 및 음성 합성 장치를 제어한다. 
- **Java 3D** 는 개발자가 단순한 상위 레벨 프로그래밍 모델을 지원하는 오브젝트 지향 인터페이스 세트를 제공하여 확장 가능한 플랫폼 독립적인 3D 그래픽을 Java 애플리케이션에 쉽게 통합하는 데 사용할 수 있는 API이다. 
- **Annotation** 을 사용하면 개발 도구, 전개 도구 또는 런타임 라이브러리에서 특별한 방법으로 처리할 수 있도록 클래스, 인터페이스, 필드 및 메소드가 특정 속성을 가지고 있다고 표시할 수 있다. 
- **Java Content Repository API** 는 구현과는 독립적으로 Java SE의 컨텐츠 저장소에 액세스하는 데 필요한 API이다. 컨텐츠 저장소는 기존 데이터 저장소의 수퍼세트인 상위 레벨 정보 관리 시스템이다. 
- **Enumeration은** 유형 안정성을 유지한 상태로 데이터의 특정 부분을 상수로 표시할 수 있는 유형이다. 
- **Generic** 을 사용하면 인스턴스화 수행 시 지정하는 추상 유형 매개변수로 클래스를 정의할 수 있다. 
- **Concurrency Utility** 는 동시 프로그램에 일반적으로 필요한 기능을 제공하는 중간 레벨 유틸리티 세트이다. 
- **JAXP(Java API for XML Processing)**를 사용하면 Java 애플리케이션이 특정 XML 처리 구현과 독립적으로 XML 문서를 구문 분석하고 변환할 수 있고 애플리케이션 코드를 변경하지 않고 XML 프로세서 간 스왑을 수행할 수 있다. **JAXB(Java API for XML Binding)**를 사용하면 XML 문서와 Java 오브젝트 간 맵핑을 자동화할 수 있다. 
- **SAAJ(SOAP with Attachments API for Java)**를 사용하면 개발자가 SOAP 1.1 스펙 및 SOAP with Attachments 참고사항을 준수하는 메시지를 생성하고 이용할 수 있다. 
  

### J2EE

- **EJB(Enterprise JavaBean)** 기술에서는 컴포넌트 모델을 사용하여 트랜잭션, 보안 및 데이터베이스 연결과 같은 서비스에 대한 자동 지원을 통해 미들웨어 애플리케이션의 개발을 단순화한다. 
- **Portlet Specification** 은 집계, 개인화, 프리젠테이션 및 보안 영역에 대해 언급하는 Java 포털 컴퓨팅을 위한 API 세트를 정의한다. 
- **JavaMail** 은 메일 시스템을 모델링하는 추상 클래스 세트를 제공하는 API이다. 
- **JMS(Java Message Service)**는 모든 JMS 기술 준수 메시징 시스템에 대한 공통 메시징 개념 및 프로그래밍 전략 세트를 정의하여 Java 플랫폼을 위한 이동형 메시지 기반 애플리케이션을 개발할 수 있는 API이다. 
- **JSF(JavaServer Faces)**는 페이지에서 재사용 가능한 UI 컴포넌트를 어셈블하고 이러한 컴포넌트를 애플리케이션 데이터 소스에 연결한 후 클라이언트가 생성한 이벤트를 서버측 이벤트 핸들러에 연결하여 웹 애플리케이션을 만드는 데 도움을 주는 프로그래밍 모델을 제공한다. 
- **JSP(JavaServer Pages)**를 사용하면 설계자가 동적 컨텐츠를 변경하지 않고 페이지 레이아웃을 변경할 수 있도록 개발자는 별도의 사용자 인터페이스 및 컨텐츠 생성을 통해 플랫폼 독립적인 동적 웹 페이지를 신속하게 개발하고 편리하게 관리할 수 있다. 이 기술은 페이지의 컨텐츠를 생성하는 로직을 캡슐화하는 XML 유사 태그를 사용한다. 
- **JSTL(Standard Tag Library for JavaServer Pages)**은 표준화된 형식으로 다수의 공통 웹 사이트 함수를 사용할 수 있도록 하는 사용자 정의 태그의 콜렉션이다. 
- **Java Servlet** 은 CGI 프로그램의 성능 제한 없이 웹 기반 애플리케이션을 빌드하는 데 필요한 플랫폼 독립적인 컴포넌트 기반 메소드를 제공하여 웹 서버의 범위를 확장한다. 
- **JCA(J2EE Connector Architecture)**는 확장 가능하고 안전한 트랜잭션 메커니즘 세트를 정의하여 J2EE 플랫폼을 이기종 EIS(Enterprise Information System)에 연결하는 데 필요한 표준 아키텍처를 정의하여 EIS 벤더가 애플리케이션 서버에 연결하는 표준 자원 어댑터를 제공할 수 있게 한다. 
- **JMX(J2EE Management Specification)**는 J2EE 플랫폼의 관리 정보 모델을 정의한다. J2EE 관리 모델은 다수의 관리 시스템 및 프로토콜과 상호 운영 가능하도록 설계되어 있고 서버 상주 EJB 컴포넌트(J2EE MEJB(Management EJB) 컴포넌트)를 통한 Java 오브젝트 모델, SNMP MIB(Management Information Base) 및 CIM(Common Information Model)에 대한 모델의 표준 맵핑을 포함하고 있다. 
- **JTA(Java Transaction API)**는 애플리케이션 및 애플리케이션 서버가 트랜잭션에 액세스할 수 있도록 하는 구현 및 프로토콜과는 독립적인 상위 레벨 API이다. **JTS(Java Transaction Service)**는 API 아래의 레벨에서 OMG OTS(Object Transaction Service) 1.1 스펙의 Java 맵핑을 구현하고 JTA를 지원하는 트랜잭션 관리자의 구현을 지정한다. JTS는 IIOP(Internet Inter-ORB Protocol)를 사용하여 트랜잭션을 전파한다. 
  

### J2ME

- **MIDP(Mobile Information Device Profile)**는 자원이 제한된 모바일 정보 장치를 위해 Java 런터임 환경을 구성하는 두 가지 구성 중 하나이다. MIDP는 사용자 인터페이스, 네트워크 연결, 로컬 데이터 스토리지 및 애플리케이션 라이프사이클 관리를 포함한 핵심 애플리케이션 기능을 제공한다. 
- **CDC(Connected Device Configuration)**는 네트워크가 연결된 이용자 및 임베디드 장치의 범위에서 공유될 수 있는 애플리케이션을 빌드하고 전달하는 데 필요한 표준 기반 프레임워크이다. 
- **M3G(Mobile 3D Graphics API for J2ME)**는 J2ME 및 MIDP와 나란히 선택적 패키지로 제공되는 대화식 경량 3D 그래픽 API이다.  

