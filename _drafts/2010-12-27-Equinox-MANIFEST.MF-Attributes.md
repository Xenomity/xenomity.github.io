---
title: "Equinox MANIFEST.MF Attributes"
tags: [eclipse, equinox, osgi]
date: 2010-12-27T16:03:26+09:00
---

META-INF/MANIFEST.MF  
  
Bundle-ManifestVersion : 번들의 OSGi 버전 호환성 명시. (R4:2)  
Bundle-Name : 번들의 이름 명시.  
Bundle-SymbolicName : 번들의 유니크한 이름 명시.  
Bundle-Version : 번들의 버전 명시.  
Bundle-Activator : 번들의 액티베이터 명시.  
Bundle-ActivationPolicy : lazy. 번들의 늦은 로딩 명시.  
Bundle-RequiredExecutionEnvironment : 번들의 실행가능 환경 명시.  
Bundle-ClassPath : 번들 내부의 리소스 또는 Jar 명시.  
Import-Package : 다른 번들의 패키지를 이용하기 위한 명시.  
Export-Package : 외부로 공개하는 패키지 명시.  
Require-Bundle : 번들이 실행되기 위한 의존 번들들 명시.  
Fragment-Host : Host Bundle Symbolic Name.  
Web-ContextPath : Context Root (Only SpringDM)

