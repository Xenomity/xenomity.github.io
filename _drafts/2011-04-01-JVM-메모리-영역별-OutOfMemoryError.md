---
title: "JVM 메모리 영역별 OutOfMemoryError"
tags: java
date: 2011-04-01T14:50:14+09:00
---

프로젝트를 하다보면 다양한 환경에서 OutOfMemoryError가 발생하고 이에 대한 대처를 단순히 JVM의 Heap 크기를 늘린다던가, 주기적인 시스템 재시작으로 운영하는 경우를 많이 보았다..-0-  
특히나 웹 프로젝트에서 Instance Pooling, Auto Re-loading 등의 기능을 지원하는 프레임워크를 이용한다거나 기타 등등의 경우도 해당 환경이 요구하는 정확한 메모리 가용량을 계산하는것이 가장 좋지만, 불안전하거나 잘못된 소스 코드에 의한 메모리 누수가 아니라는 가정 하에 JVM 메모리 튜닝을 위한 내용을 간단히 정리해 보았다.  
  
JVM 메모리 구조에 대한 간략한 설명은 [이전 포스팅](http://www.xenomity.com/entry/JVM-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0) 참고.

#### **1. Heap 영역**

|  java.lang.OutOfMemoryError:  
 java heap space |  Heap 영역의 크기 부족으로 Object를 생성하지 못하는 경우 |
|  java.lang.OutOfMemoryError:  
 Requested array size exceeds VM limit |  Heap 영역의 최대 크기보다 큰 배열을 생성하려는 경우 |

이 경우, 메모리 누수가 발생하지 않는 상황이라면 Application이 필요로하는 메모리 크기보다 Heap 영역의 크기가 부족하다는 결론을 얻게된다. jvm argument로 원하는 크기의 Heap 사이즈를 명시하여 문제를 해결한다.  
  
예) java -Xmx256m TestApp  
  
반대로, 메모리 누수가 발생하는 경우라면 경험상으로는 거의 90% 이상(ㅋ..)이 소스 코드의 문제였다. 자원을 물고 놓지 않는다던가 불필요한 객체 생성 등등의 이유로 Heap 영역의 메모리 사용량은 점점 늘어나게 되고, 결국은 OOME를 뱉어내며 장렬히 전사하신다. ㅡ,.ㅡ;; 코드 튜닝 하도록~ -0-  
(물론 WAS의 버그나 JDK의 버그에 의한 것일수도 있지만, 극히 소수임.)  
  
정확한 프로파일링을 위해서 jvm의 hprof 옵션을 사용해보는 것을 추천한다.  
  
예) java -Xhprof:heap=sites TestApp  

rank   self   accum   bytes   objs   bytes   objs   trace   name  
 1    19.43% 20.36%  220060     16  190060     16  300000java.lang.String  
 2    10.12% 35.28%  139260   1059  139260   1059  300000  char[]  
 3    10.01% 40.56%  130192     15   49192     15  300055  byte[]  
 4     2.26% 45.82%   49112     14   49112     14  300066  byte[]  
...  

위 TestApp의 경우 String 객체가 19%의 Heap 영역을 사용하는것을 알 수 있다. ;)  
  
  

#### **2. Permanent 영역**

|  java.lang.OutOfMemoryError:  
 Perm Gen space |  많은 Class들이 로드되는 경우 |

이 경우, 웹 어플리케이션이나 동적으로 클래스를 로드하고 풀링하는 등의 프레임웍을 사용할 때 자주 나타난다. 왜냐하면 수많은 jsp/servlet도 결국 클래스이며, 프레임워크에서도 자동으로 클래스 로딩 작업이 주기적으로 일어나기 때문이다. (auto reloading 등...)  
Perm 영역의 크기를 조절하여 문제를 해결한다.  
  
예) java -XX:PermSize=128m -XX:MaxPermSize=256m  
  
JConsole을 통해 클래스 로드 상황을 모니터링할 수도 있다.  
  
  

#### **3. Native 영역**

|  java.lang.OutOfMemoryError:  
<font face="Courier New"> request bytes for . Out of swap space?</font> |  VM code-level에서 메모리 부족이 발생한 경우  
 Swap 크기가 부족한 경우 |
|  java.lang.OutOfMemoryError:  
<font face="Courier New"> (Native Method)</font> |  JNI/Natvie Method에서 메모리 부족이 발생한 경우 |
|  java.lang.OutOfMemoryError:  
<font face="Courier New"> unable to create new native thread</font> |  Thread 생성 공간이 부족한 경우 |

<font style="BACKGROUND-COLOR: #ffffff" color="#d18e0a">(1번째의 경우는 아직 한번도 보질 못해서 생략... ^^;;)</font>  
  
Native 영역에서 발생한 OOME의 경우에는 친절(?)하게도 JVM은 FatalError Log를 날려준다.ㅋ  
  
두번째의 경우는 JNI를 통한 native lib(dll, so 등)들을 사용하는 경우 발생한다. Native 언어로 작성된 코드를 적절히 수정할 필요가 있다.  
  
세번째의 경우는 Thread가 많이 사용되거나 Thread Stack의 크기가 큰 경우, Heap 영역이 너무 커서 상대적으로 할당되는 Native 영역이 부족하여 발생한다. Java에서 생성되는 Thread는 OS에서 할당되는 Thread Stack 영역을 차지하므로, Native 메모리 영역을 튜닝할 필요가 있다.  
  

Java Process가 사용 가능한 공간 = Heap 크기 + Perm 크기 + Native 크기  

  
jvm의 -Xss 옵션을 통해 Thread Stack 영역의 크기를 조절할 수도 있지만, 역으로 StackOverflowError가 발생할 확률이 높아지기에, 가장 이상적인 방법은 적절하게 Heap 영역의 크기를 최소화하는 방법이다.  
(아직까지도 가용범위 내에서 Heap 크기를 불필요하게 늘려 운영하는 프로젝트들을 여럿 보았다...;;)  
  
참고로 Process가 이용가능한 메모리 크기의 제한이 없는 64bit JVM을 이용하는 것도 방법이 될 수 있다.

