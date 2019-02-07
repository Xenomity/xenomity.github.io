---
title: "Java Application을 Java Web Start (JWS)로 전개하기"
tags: [java, jws]
date: 2011-03-21T20:07:47+09:00
---
  
ActiveX의 대안으로 Pure Java Application을 개발하여 Java Web-Start를 통한 전개는 개인적으로 좋은 방법 중 하나라고 생각한다. 엔드 유저에게도 이질감없이 받아들일 수 있는 사용자 경험을 제공하며, 보안측면에서도 SandBox 내에서 code-safe한 동작이 보장된다.  
  
일례로, 예전에 공인인증 모듈을 개발한 적이 있었는데, 처음에는 RCP로 개발했다가 용량면에서 부담스러운 부분이 생기더라..; (Required Bundle을 포함하여 압축된 Jar로 패키징해도 최소10MB가 훌쩍 뛰어넘는다..-0-)  
결국 사랑하는(?) SWT만을 이용하여 개발하였다..ㅇ,.ㅇ  
  
참고로 모 공공 사이트에서도 Cross-Platform을 위해 Swing으로 개발된 공인인증 모듈이 적용된 것을 확인할 수 있었다.  
  
JWS 엔진은 최초에 클라이언트의 jre 버전을 확인하고, 없거나 충족되지 못하면 Descriptor에 기술된 정보대로 jre 환경의 자동설치, 모듈의 버전변경에 따른 자동 업데이트, 캐싱 등등 다양한 기술을 제공한다.  
  
  

#### **1. Application을 Runnable-Jar로 패키징**
배포될 어플리케이션을 실행 가능한 단일 Jar로 패키징한다.  
(단일 Jar로 패키징하지 않으면 일일히 각각 Jar마다 서명해야 하는 귀차니즘이 생기기에.;;)  
Eclipse Platform의 JarInJar ClassLoader를 이용하면 JRE 구 버전에서도 Jar 내부의 Jar를 쉽게 참조할 수 있다. 아님 직접 ClassLoader를 구현하여도 되지만, 그게 아니라면 필수 Jar들을 전부 풀어서 다시 Re-packaging해야 하는 번거로움이 생길수도 있다.  
  
예) Export -\> Java -\> Runnable Jar File  

[##\_1C|cfile7.uf.230C3B3C530196D003F274.jpg|width="590" height="352" filename="201103211748.jpg" filemime="image/jpeg"|\_##]

  
- Launch Configuration  
Main method가 존재하는 실행 가능한 클래스와 해당 프로젝트 선택.  
  
- Library Handling  
1) Extract required libraries into generated JAR : 필수 라이브러리들을 전부 풀어서 리패키징.  
2) Package required libraries into generated JAR : 필수 라이브러리들을 전부 Jar 내부로 포함하여 패키징.  
3) Copy required libraries into sub-folder next to the generated JAR : 필수 라이브러리들을 전부 Jar 외부의 하위 경로로 배포하고 class-path로 참조.  
  
서명을 위해서는 1 or 2번 방법이 가장 심플하다.  
  
<font color="#f3d756">* 참고로 SWT Application의 경우, Eclipse 3.5 이상 버전에 번들되어 있는 버전으로 교체함으로써 native library들을 별도로 배포하고 path를 잡아줘야 하는 번거로운 문제들을 해결할 수 있다. </font>  
  
  

#### **2. Signed-Jar 생성**
VeriSign 등의 인증 업체를 통해 생성된 Runnable-Jar를 서명한다. 서명을 하지 않으면 당근 브라우저의 보안 레벨 설정에 따라 설치 가부가 결정되게 된다.  
(테스트로 local key-store를 생성하고 jarsigner로 서명할수도 있다.)  
  
  

#### **3. JNLP Descriptor 작성**
JWS 구성을 위한 jnlp 기술자를 작성한다.  
  
예) jnlp 1.5 Spec  

    \<?xml version="1.0" encoding="utf-8"?\> \<!-- where the jnlp file lives on the web --\> \<jnlp spec="1.5+" codebase="http://xenomity.com" href="test.jnlp" version ="1.9"\>     \<information\>         \<title\>Title\</title\>         \<vendor\>Vendor\</vendor\>         \<homepage href="webstart/test.html" /\>         \<description\>Description.\</description\>         \<description kind="short"\>Short Description.\</description\>         \<description kind="tooltip"\>Tooltip Description\</description\>         \<!-- gif or jpg only, no pngs in 1.5. Transparency does not work. Rectangular icons will be badly stretched. --\>         \<!-- relative to codebase --\>         \<icon href="../images/espericon64.gif" width="64" height="64" kind="default" /\>         \<icon href="../splash.gif" kind="splash" /\>         \<!-- allow app to run without Internet access --\>         \<offline-allowed /\>         \<!-- request that the JAWS app be hooked up as the official OS handler of a given file type --\>         \<!-- note the plural extensions, but always a single value --\>         \<association mime-type="application/rtf" extensions="rtf" /\>         \<association mime-type="image/wavelet" extensions="wi" /\>         \<!-- hints for setting up shortcuts --\>         \<!-- Prefer a shortcut for online operation --\>         \<shortcut online="true"\>         \<!-- create desktop shortcut --\>         \<desktop /\>         \<!-- create menu item for this app under the major heading Esperanto --\>         \<menu submenu="Esperanto" /\>         \</shortcut\>     \</information\>     \<security\>         \<all-permissions /\>         \<!-- all-permissions requires the jars be signed --\>     \</security\>     \<resources\>         \<!-- Acceptable JVMs in preferred order, best first --\>         \<!-- Sun JVM --\>         \<j2se version="1.6.0\_17" href="http://java.sun.com/products/autodl/j2se" java-vm-args="-ea" initial-heap-size="128m" max-heap-size="512m" /\>         \<j2se version="1.6+"href="http://java.sun.com/products/autodl/j2se" java-vm-args="-ea" initial-heap-size="128m" max-heap-size="512m" /\>         \<!-- allow any vendor --\>         \<j2se version="1.6+" java-vm-args="-ea" initial-heap-size="128m" max-heap-size="512m" /\>         \<j2se version="1.5+" href="http://java.sun.com/products/autodl/j2se" java-vm-args="-ea" initial-heap-size="128m" max-heap-size="512m" /\>         \<!-- allow any vendor --\>         \<j2se version="1.5+" java-vm-args="-ea" initial-heap-size="128m" max-heap-size="512m" /\>         \<!-- application code, load before launch. JNLP 1.6 main="true" indicates jar with main class --\>         \<jar href="esper.jar" main="true" download="eager" size="99999" /\>         \<!-- data dictionaries in compressed form, load as needed. --\>         \<jar href="dicts.jar" main="false" download="lazy" size="99999" /\>         \<!-- aux JNLP to describe the installer --\>         \<extension name="Installer" href="esperinstaller.jnlp"/\>         \<!-- set a -D system property --\>         \<property name="flavour" value="strawberry" /\>     \</resources\>     \<!-- JNI native Sun .so code --\>     \<resources os="SunOS" arch="sparc"\>         \<!-- relative to codebase --\>         \<nativelib href="lib/solaris/corelibs.jar" /\>     \</resources\>     \<!-- JNI native Windows .dll code --\>     \<resources os="Windows" arch="x86"\>         \<!-- relative to codebase --\> nativelib href="lib/windows/corelibs.jar" /\>     \</resources\>     \<!-- application class with main method --\>     \<application-desc main-class="com.mindprod.esper.Esperanto"\>     \<!-- command line arguments --\>         \<argument\>Esperanto\</argument\>         \<argument\>English\</argument\>     \</application-desc\>     \<!-- \<applet-desc would go here for applet --\>     \<!-- code run once on install to unpack dicts.jar as part of one-time install --\>     \<!-- this has to go in a separate JNLP file from application-desc. --\>     \<installer-desc main-class="com.xenomity.package.Installer" /\> \</jnlp\>

  
정적인 문서이므로 동적인 파라메터를 어플리케이션으로 전달해야 할 경우, jsp로 작성할 수도 있겠다..ㅎㅎ  
  
예) JSP by jnlp (MIME type :  <font face="Courier New"><span style="FONT-FAMILY: Gulim">application/x-java-jnlp-file)</span></font>

    \<%@page contentType="application/x-java-jnlp-file" pageEncoding="utf-8"%\> \<?xml version="1.0" encoding="utf-8"?\> \<jnlp spec="1.0+" codebase="\<%=request.getAttribute("remoteURL") %\>" \> \<information\>   \<title\>세금계산서 발행\</title\>   \<vendor\>\*\*\*\*\*\*\</vendor\>   \<description\>세금계산서 발행\</description\>   \<description kind="short"\>세금계산서 발행\</description\>   \<icon href="/taxasp/static/images/common/other/splash\_rca.gif" kind="splash" /\> \</information\> \<security\>   \<all-permissions/\> \</security\> \<resources\>   \<j2se version="1.5+" href="http://java.sun.com/products/autodl/j2se" /\>   \<jar href="./rca.jar" /\> \</resources\> \<application-desc main-class="com.htis.pub.Application"\>   \<argument\>\<%=request.getAttribute("param1") %\>\</argument\>   \<argument\>\<%=request.getAttribute("param2") %\>\</argument\> \</application-desc\> \</jnlp\>

  
  

#### **4. 테스트**
작성한 jnlp 기술자와 서명된 jar를 원하는 경로로 배포하고 jnlp를 직접 실행하여본다.  
  

    ...[실행](**/*/rca.jnlp)...

  

[##\_1C|cfile24.uf.224F1739530196E43AEFF2.jpg|width="519" height="302" filename="201103211947.jpg" filemime="image/jpeg"|\_##]

- Java Web Start Splash 화면 -  
  

[##\_1C|cfile24.uf.2577143B530196F407C868.jpg|width="518" height="397" filename="201103211953.jpg" filemime="image/jpeg"|\_##]

- 브라우저를 통해 전개된 Application의 예 -  
  
  

  

