---
title: "Android System Properties"
tags: android
date: 2011-02-03T10:56:44+09:00
---
  
<font color="#d18e0a">* Android 2.2 Proyo 기준</font>.  
  
1) System Properties Key 리스트  

|  <font color="#000000">java.vm.version</font> |  <font color="#000000">java.version</font> |  <font color="#000000">java.ext.dirs</font> |
|  <font color="#000000">java.vendor.url</font> |  <font color="#000000">java.boot.class.path</font> |  <font color="#000000">java.class.path</font> |
|  <font color="#000000">java.vm.vendor.url</font> |  <font color="#000000">java.library.path</font> |  <font color="#000000">os.version</font> |
|  <font color="#000000">user.dir</font> |  <font color="#000000">file.separator</font> |  <font color="#000000">java.specification.name</font> |
|  <font color="#000000">java.vm.name</font> |  <font color="#000000">java.specification.vendor</font> |  <font color="#000000">java.compiler</font> |
|  <font color="#000000">java.home</font> |  <font color="#000000">file.encoding</font> |  <font color="#000000">user.language</font> |
|  <font color="#000000">user.region</font> |  <font color="#000000">line.separator</font> |  <font color="#000000">user.name</font> |
|  <font color="#000000">javax.net.ssl.trustStore</font> |  <font color="#000000">java.vm.specification.version</font> |  <font color="#000000">os.arch</font> |
|  <font color="#000000">java.runtime.name</font> |  <font color="#000000">java.vm.specification.vendor</font> |  <font color="#000000">java.runtime.version</font> |
|  <font color="#000000">user.home</font> |  <font color="#000000">os.name</font> |  <font color="#000000">java.class.version</font> |
|  <font color="#000000">java.io.tmpdir</font> |  <font color="#000000">java.vm.vendor</font> |  <font color="#000000">java.vendor</font> |
|  <font color="#000000">http.agent</font> |  <font color="#000000">path.separator</font> |  <font color="#000000">java.vm.specification.name</font> |
|  <font color="#000000">java.net.preferIPv6Addresses</font> |  <font color="#000000">android.vm.dexfile</font> |  <font color="#000000">java.specification.version</font> |

  
2) 내 환경 예  

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

  
