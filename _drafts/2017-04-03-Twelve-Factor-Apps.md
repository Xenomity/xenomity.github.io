---
title: "Twelve Factor Apps"
date: 2017-04-03 08:26:28
tags: [fundmental]
---

# Twelve Factor Apps
'**Twelve-Factor**'란, 클라우드 환경에서 좀 더 나은 마이크로-서비스 애플리케이션을 설계하기 위한 개발 방법론이다. 다양한 SaaS 환경에서 관찰된 설계와 시스템의 문제를 공유하고, 유비쿼터스 언어로 통용화하기 위해 12가지 용어로 정리되었다. 이를 준수하여 개발하는 서비스 또는 애플리케이션을 여기서는 'Twelve factor app'이라 부른다.  
대충 훑어보면 대게 당연한 원리-원칙이지만, 가끔씩 아키텍쳐 관점이 흐려질 때 환기시키기 좋은 기준이 될 수 있다.

## 12 factors
### Codebase
애플리케이션 코드는 단일 저장소에서 유지/통합될 수 있어야 하고, 별도의 리비전이나 코드의 변경 없이 다양한 환경으로 배포될 수 있어야 한다. 단일 코드로 모든 환경에서 서비스가 가능하도록 도메인 로직과 시스템에 종속적인 설정 정보를 분리하는 것이 관건이며, 이를 통해 애플리케이션 코드의 일관성을 보장한다.
이 원칙이 잘 지켜지지 않으면 저장소의 코드베이스는 배포되는 환경에 따라 별도의 트리가 생기는 등, 코드의 복잡성과 유지보수 비용이 증가하게 된다.

### Dependencies
애플리케이션이 필요로 하는 모든 의존적 패키지는 선언적으로 관리될 수 있어야 한다. 또한 코드베이스에 번들되는 형식이 아닌, 별도의 저장소로 분리하여 관리되어야 한다. 이미 Maven 또는 Gradle 등 많은 프로젝트 관리 도구들이 이러한 고립적인 의존성 관리 기능을 제공하고 있으며, 릴리즈 시점에 해당 의존성 패키지들은 애플리케이션 패키지와 자연스럽게 통합된다.

### Config
애플리케이션의 설정 정보는 각 환경에 따라 별도로 관리할 수 있고 자동화해야 한다. 설정 정보를 코드베이스로 통합하거나 정적으로 관리할 경우, 실행 환경에서 동적인 설정 변경이나 시스템 확장이 발생하게 되면 최악의 경우 애플리케이션의 재-빌드, 재-배포가 필요할 수 있으며 시스템 전반적으로 가용성과 유연성을 떨어뜨리게 된다.

### Backing services
서비스 상호 또는 서비스가 이용하는 모든 시스템은 투명하게 연결될 수 있어야 한다. 연계 시스템은 표준화된 인테페이스를 노출하여 프로토콜에 종속적이지 않은 느슨한 연결이 가능해야 하며, 서비스는 설정 정보를 통해 연계 시스템의 변경이 발생하여도 코드의 수정 없이 유연하게 동작할 수 있어야 한다. 네트워크 구간 내에서 협응하는 시스템이 무엇인지 애플리케이션 코드는 알 필요가 없다.

### Build, release, run
빌드와 릴리즈, 배포 및 실행 과정은 엄격하게 분리되어야 한다. 명확한 버전 관리를 위해서는 각 단계별로 최대한 변화가 적고 순차적이어야 하며, 만약 코드가 수정된다면 다시 빌드 단계를 거치도록 하여 전반적인 프로세스에 일관성이 흐트러지지 않도록 한다. 이 원칙은 실행 단계에 배포된 패키지가 문제를 일으켜도, 쉽고 빠르게 원하는 버전으로 롤백할 수 있는 기반을 제공한다.

### Processes
가능하다면 애플리케이션은 하나 혹은 여러개의 무-상태(_stateless_) 프로세스로 실행되어야 한다. 서비스의 모든 정보는 목적에 따라 영속화하고, 애플리케이션은 언제든지 이 정보를 기반으로 최신 상태의 서비스가 가능해야 한다. 만약 프로세스가 상태-유지(_stateful_)한 경우, 환경이 변화함에 따라 인-메모리 상의 정보를 동기화하거나 장애 전파를 최소화하기 위한 가용성 유지 작업에 시간과 노력이 들어가게 될 뿐더러, 여러모로 시스템의 확장에 제약적인 영향을 미친다.

### Port binding
서비스가 특정 컨테이너나 인프라 스트럭쳐 또는 실행 환경이 제공하는 기술에 의존적으로 기능을 노출하게 된다면 애플리케이션의 이식성과 독립성이 떨어질 수 밖에 없다. 단위 프로세스는 어떠한 실행 환경의 런타임에도 의존하지 않고 독자적인 서비스가 가능하도록 포트 바인딩을 통해 원하는 기능을 제공할 수 있어야 한다.

### Concurrency
애플리케이션의 규모가 크거나 주-도메인에 독립적으로 제공되는 부수 기능(_e.g. sending email, tweet, batch task..._)들이 다수 포함된다면, 별도의 작은 프로세스로 분리하여 동시성과 수평 확장성의 이득을 꾀할 수 있다. 각 프로세스는 독립적으로 시스템 자원을 점유할 수 있고, 무-상태 프로세스의 수평적 분할은 동시성을 높게 하며, 주-서비스의 동작에 장애를 전파할 확률이 거의 없어 애플리케이션을 좀 더 견고하게 만들고, 자연스레 관심사가 분리가 이루어진다.

### Disposability
서비스는 가능한 빠르게 기동되어야 하고 우아하게 종료할 수 있는 매커니즘을 통해 시스템의 안전성을 확보할 수 있어야 한다. 서비스의 기동 속도가 짧으면 짧을수록 부하 분산이나 로드 밸런싱의 실패 시간이 줄어들고 연관 프로세스와의 협응이 민첩하게 이루어질 수 있다. 그리고 명시적인 종료 절차를 통해 자원 해제와 서비스 상태 변경 등의 과정을 거쳐 장애 전파를 최소화하고 시스템에 잔존하는 위험성을 제거한다.  

### Dev/prod parity
각 실행 환경은 최대한 유사하게 구성한다. 애플리케이션의 의존성 목록과 설정 정보는 환경별로 분리하여 관리하고, 지속적인 빌드/배포 프로세스를 확립하여 코드베이스의 일관성을 보장할 수 있도록 한다.

### Logs
모든 로그는 이벤트 스트림처럼 취급되어야 한다. 실시간으로 누적되는 로그를 규칙에 따라 필터링하고 목적(_e.g. error reporting, data analysis..._)에 맞게 가공하여 영속화하는 것이 일반적이며, 이러한 과정은 별도의 로그 수집 시스템을 구성하여 메인 서비스와의 의존성을 제거하고, 지속적인 분석과 가공을 통해 다양한 방법으로 시각화된다.

### Admin processes
관리용 작업은 일회성 프로세스로 실행될 수 있어야 한다. 실행 환경에서 -_데이터의 불일치를 해결하거나 더미 데이터를 주기적으로 삭제하는 것과 같은_- 일련의 유지보수용 작업의 경우, 개발자가 해당 환경의 터미널이나 저장소에 직접 접근하여 제어하는 행동은 많은 실수와 문제를 야기시킨다. 목적에 맞는 관리 도구를 주-애플리케이션과 같은 코드베이스, 같은 설정 정보를 바탕으로 작성하고, 버전의 불일치를 방지하게 위하여 동일한 빌드, 배포 프로세스를 거쳐 실행 환경에서 구동할 수 있도록 한다.

## References
- [12-Factor Apps in Plain English](http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/)