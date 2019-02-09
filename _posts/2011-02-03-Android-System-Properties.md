---
title: "Android System Properties"
tags: android
date: 2011-02-03T10:56:44+09:00
---
  
<font color="#d18e0a">* Android 2.2 Proyo 기준</font>.  
  
1) System Properties Key 리스트  

- java.vm.version
- java.version
- java.ext.dirs
- java.vendor.url
- java.boot.class.path
- java.class.path
- java.vm.vendor.url
- java.library.path
- os.version
- user.dir
- file.separator
- java.specification.name
- java.vm.name
- java.specification.vendor
- java.compiler
- java.home
- file.encoding
- user.language
- user.region
- line.separator
- user.name
- javax.net.ssl.trustStore
- java.vm.specification.version
- os.arch
- java.runtime.name
- java.vm.specification.vendor
- java.runtime.version
- user.home
- os.name
- java.class.version
- java.io.tmpdir
- java.vm.vendor
- java.vendor
- http.agent
- path.separator
- java.vm.specification.name
- java.net.preferIPv6Addresses
- android.vm.dexfile
- java.specification.version

  
2) 내 환경 예  
```
System.getProperties().list(System.out);

02-03 01:42:20.466: INFO/System.out(430): java.vm.version=1.2.0  
02-03 01:42:20.466: INFO/System.out(430): java.vendor.url=http://www.android.com/  
02-03 01:42:20.476: INFO/System.out(430): java.vm.vendor.url=http://www.android.com/  
02-03 01:42:20.476: INFO/System.out(430): user.dir=/  
02-03 01:42:20.486: INFO/System.out(430): java.vm.name=Dalvik  
02-03 01:42:20.486: INFO/System.out(430): java.home=/system  
02-03 01:42:20.496: INFO/System.out(430): user.region=KR  
02-03 01:42:20.496: INFO/System.out(430): javax.net.ssl.trustStore=/system/etc/security/cacerts.bks  
02-03 01:42:20.505: INFO/System.out(430): java.runtime.name=Android Runtime  
02-03 01:42:20.505: INFO/System.out(430): user.home=  
02-03 01:42:20.505: INFO/System.out(430): java.io.tmpdir=/sdcard  
02-03 01:42:20.505: INFO/System.out(430): http.agent=Dalvik/1.2.0 (Linux; U; Android 2.2; ...  
02-03 01:42:20.505: INFO/System.out(430): java.net.preferIPv6Addresses=true  
02-03 01:42:20.516: INFO/System.out(430): java.version=0  
02-03 01:42:20.516: INFO/System.out(430): java.boot.class.path=/system/framework/core.jar:/system/fr...  
02-03 01:42:20.516: INFO/System.out(430): java.library.path=/system/lib  
02-03 01:42:20.516: INFO/System.out(430): file.separator=/  
02-03 01:42:20.516: INFO/System.out(430): java.specification.vendor=The Android Project  
02-03 01:42:20.516: INFO/System.out(430): file.encoding=UTF-8  
02-03 01:42:20.526: INFO/System.out(430): line.separator=  
02-03 01:42:20.526: INFO/System.out(430): java.vm.specification.version=0.9  
02-03 01:42:20.526: INFO/System.out(430): java.vm.specification.vendor=The Android Project  
02-03 01:42:20.536: INFO/System.out(430): os.name=Linux  
02-03 01:42:20.536: INFO/System.out(430): java.vm.vendor=The Android Project  
02-03 01:42:20.546: INFO/System.out(430): path.separator=:  
02-03 01:42:20.546: INFO/System.out(430): android.vm.dexfile=true  
02-03 01:42:20.546: INFO/System.out(430): java.ext.dirs=  
02-03 01:42:20.556: INFO/System.out(430): java.class.path=.  
02-03 01:42:20.556: INFO/System.out(430): os.version=2.6.29-00261-g0097074-dirty  
02-03 01:42:20.556: INFO/System.out(430): java.specification.name=Dalvik Core Library  
02-03 01:42:20.556: INFO/System.out(430): java.compiler=  
02-03 01:42:20.566: INFO/System.out(430): user.language=ko  
02-03 01:42:20.566: INFO/System.out(430): user.name=  
02-03 01:42:20.566: INFO/System.out(430): os.arch=armv5tejl  
02-03 01:42:20.566: INFO/System.out(430): java.runtime.version=0.9  
02-03 01:42:20.566: INFO/System.out(430): java.class.version=46.0  
02-03 01:42:20.566: INFO/System.out(430): java.vendor=The Android Project  
02-03 01:42:20.566: INFO/System.out(430): java.vm.specification.name=Dalvik Virtual Machine Specification  
02-03 01:42:20.576: INFO/System.out(430): java.specification.version=0.9  
```
  
