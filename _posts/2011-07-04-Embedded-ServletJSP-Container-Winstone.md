---
title: "Embedded Servlet/JSP Container Winstone"
tags: winstone
date: 2011-07-04T21:44:00+09:00
---

Java DB나 이전 Derby, HSQL처럼 Embeded형 DB는 여럿 종류를 보았지만, Embedded형 Servlet/JSP Container는 Winstone이 독보적인 것으로 알고있다. 현재 Hudson CI가 WAS 없이도 번들된 Winstone을 통해 웹서비스가 가능한 상태로 실행되며, 속도 또한 상당히 빠릿빠릿하다. Jetty와 비교될 정도로 가볍다.-0-  
이력을 보면 처음 릴리즈된건 꾀 오래 전인데... 2008년을 마지막으로 더이상의 업데이트는 없는 상태이다.  
  
실제 어느 정도의 요청을 수용할 수 있는지는 정보가 많지 않다. 단 Container 없이도 가볍고 빠르게 웹 어플리케이션을 구동시킬 수 있다는 것이 가장 큰 장점이다. 또한 확장성도 좋아 다양한 방식으로 번들화 및 패키징이 가능하다.  
  
  
예 1) Command-Line Options
```
Syntax:  
  java -jar winstone-0.9.10.jar [--option=value] [--option=value] etc  
  
Required options: either --webroot OR --warfile OR --webappsDir OR --hostsDir  
   --webroot                = set document root folder.  
   --warfile                = set location of warfile to extract from.  
   --webappsDir             = set directory for multiple webapps to be deployed from  
   --hostsDir               = set directory for name-based virtual hosts to be deployed from  
  
Other options:  
   --javaHome               = Override the JAVA\_HOME variable  
   --toolsJar               = The location of tools.jar (default is JAVA\_HOME/lib/tools.jar)   
   --config                 = load configuration properties from here(if supplied).  
   --prefix                 = add this prefix to all URLs. (eg http://host/prefix/etc).  
   --commonLibFolder        = folder for additional jar files. Default is ./lib  
      
   --logfile                = redirect winstone log messages to this file  
   --logThrowingLineNo      = show the line no that logged the message (slow). Default is false  
   --logThrowingThread      = show the thread that logged the message. Default is false  
   --debug                  = set the level of debug msgs (1-9). Default is 5 (INFO level)  
  
   --httpPort               = set the http listening port. -1 to disable, Default is 8080  
   --httpListenAddress      = set the http listening address. Default is all interfaces  
   --httpDoHostnameLookups  = enable host name lookups on http connections. Default is false  
   --httpsPort              = set the https listening port. -1 to disable, Default is disabled  
   --httpsListenAddress     = set the https listening address. Default is all interfaces  
   --httpsDoHostnameLookups = enable host name lookups on https connections. Default is false  
   --httpsKeyStore          = the location of the SSL KeyStore file. Default is ./winstone.ks  
   --httpsKeyStorePassword  = the password for the SSL KeyStore file. Default is null  
   --httpsKeyManagerType    = the SSL KeyManagerFactory type (eg SunX509, IbmX509). Default is SunX509  
   --ajp13Port              = set the ajp13 listening port. -1 to disable, Default is 8009  
   --ajp13ListenAddress     = set the ajp13 listening address. Default is all interfaces  
   --controlPort            = set the shutdown/control port. -1 to disable, Default disabled  
  
   --handlerCountStartup    = set the no of worker threads to spawn at startup. Default is 5  
   --handlerCountMax        = set the max no of worker threads to allow. Default is 300  
   --handlerCountMaxIdle    = set the max no of idle worker threads to allow. Default is 50  
  
   --directoryListings      = enable directory lists (true/false). Default is true  
   --useJasper              = enable jasper JSP handling (true/false). Default is false  
   --useServletReloading    = enable servlet reloading (true/false). Default is false  
   --preferredClassLoader   = override the preferred webapp class loader.  
   --useInvoker             = enable the servlet invoker (true/false). Default is true  
   --invokerPrefix          = set the invoker prefix. Default is /servlet/  
   --simulateModUniqueId    = simulate the apache mod\_unique\_id function. Default is false  
   --useSavedSessions       = enables session persistence (true/false). Default is false  
   --usage / --help         = show this message  
      
Cluster options:  
   --useCluster             = enable cluster support (true/false). Default is false  
   --clusterClassName       = Set the cluster class to use. Defaults to SimpleCluster class  
   --clusterNodes           = a comma separated list of node addresses (IP:ControlPort,IP:ControlPort,etc)  
  
JNDI options:  
   --useJNDI                      = enable JNDI support (true/false). Default is false  
   --containerJndiClassName       = Set the container wide JNDI manager class to use. Defaults to ContainerJNDIManager  
   --webappJndiClassName          = Set the web-app JNDI manager class to use. Defaults to WebAppJNDIManager  
   --jndi.resource.\<name\>         = set the class to be used for the resource marked \<name\>  
   --jndi.param.\<name\>.\<att\>      = set an attribute \<att\> for the resource marked \<name\>  
  
Security options:  
   --realmClassName               = Set the realm class to use for user authentication. Defaults to ArgumentsRealm class  
  
   --argumentsRealm.passwd.\<user\> = Password for user \<user\>. (for ArgumentsRealm)  
   --argumentsRealm.roles.\<user\>  = Roles for user \<user\> (comma-separated) (for ArgumentsRealm)  
  
   --fileRealm.configFile         = File containing users/passwds/roles. Only valid for the FileRealm realm class  
  
Access logging:  
   --accessLoggerClassName        = Set the access logger class to use for user authentication. Defaults to disabled  
   --simpleAccessLogger.format    = The log format to use. Supports combined/common/resin/custom (SimpleAccessLogger only)  
   --simpleAccessLogger.file      = The location pattern for the log file(SimpleAccessLogger only)  
```
  
위에서 보듯, 패키징된 war는 별개 파라메터로 정의할 수 있지만, `embedded.war`라는 파일명으로 `winstone.jar` 내부에 통합시킬 수도 있다. winstone은 jar 내부의 `embedded.war`라는 파일이 존재하면 자동으로 디플로이하고 어플리케이션을 런칭시킨다. 이때는 MANIFEST.MF에 Main-Class가 `winstone.Launcher`로만 정의되어 있으면 된다.  
  
예 2) META-INF/MANIFEST.MF  
```
Manifest-Version: 1.0  
Ant-Version: Apache Ant 1.5.3   
Created-By: Apache Maven  
Built-By: rickk  
Package: winstone  
Build-Jdk: 1.5.0_11  
Extension-Name: winstone  
Specification-Title:   
Specification-Vendor:   
Implementation-Title: winstone  
Implementation-Vendor:   
Implementation-Version: 0.9.9  
Main-Class: winstone.Launcher  
```
  
예 3) 다양한 지원 사항들  
Session persistence across reboots  
Embedding Winstone  
Access Logging  
HTTPS support  
JNDI support  
Control port functions/protocol  
Cluster support  
Using Authentication Realms  
Connecting to Apache  
JSPs (aka using with Jasper)  
Using Xerces as an XML parser (required for v2.4 webapps)  
등...  

  
  
자세한 내용은 [http://winstone.sourceforge.net/](http://winstone.sourceforge.net/) 참고한다.  
  
