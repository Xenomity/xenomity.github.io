---
title: "Java Application 중복실행 방지"
tags: java
date: 2010-12-27T15:25:07+09:00
---

Java Application은 각각 별도의 jvm process로 동작하므로 상호 통신 및 정보를 주고받기에 여러가지 에로사항이 있다. 그로 인해 현재 동일한 프로그램이 실행되고 있는지를 체크하는 방법이 여러가지가 편법(?)으로 사용되고 있는데, 그 중 몇 가지를 나열해 보자면  
  
1. Database Flag 값 변경.  
2. 환경설정 파일 변경 혹은 쓰레기 파일 생성.  
3. 임의의 사용하지 않는 port 할당.  
  
이 중, 1,2번은 관리차원에서도 좋긴 하지만 한가지 문제가 발생한다. 가상머신이 critical한 문제로 강제 종료되는 현상이 발생하게 되면, gc에 의한 finallize가 호출되기 이전에 비정상적인 상태로 process가 os에서 제거되기 때문에 아무리 프로그램에서 후처리를 잘 해두었어도 데이터 복원이 불가한 상황이 되어버린다. 즉, db의 flag값이 '실행중' 이거나 환경설정 파일의 내용이 변경되었어도 비정상적으로 jvm이 terminated 되면, 데이타를 복원할 수 없다.  
그래서 본인의 경우, 3번째 방법을 자주 사용하는데 절대 사용되지 않을것같은(? ..ㅋ) 임의의 포트를 할당시키고, 그 포트가 점유되어 있는 동안은 동일 프로그램이 이미 실행중이라는 가정을 한다. 이 경우, jvm이 강제 종료되어도 점유된 port는 process가 제거되면서 함께 해제된다.

```java
/**
 * Monitor
 *
 * @author Xenomity™ a.k.a P-slinc' (이승한)
 *
 */
public class Monitor {
 
    private static DatagramSocket isRun;
 
 
    /*
     * The Constructor
     */
    private Monitor() {}
 
    /**
     * 현재 프로그램을 모니터링
     *
     * @throws MonitoringException
     */
    public static void monitoring() throws MonitoringException {
        try {
            isRun = new DatagramSocket(1103);
        } catch (SocketException ex) {
            throw new MonitoringException(ex);
        }
    }
 
    /**
     * 모니터링 종료
     */
    public static void close() {
        if(isRun != null) {
            isRun.close();
        }
    }
}
```


- Application.java
```java
// 모니터링 시작
...
 
try {
    com.xenomity.xpt.core.Monitor.monitoring();
} catch (MonitoringException e) {
    MessageDialog.openError(null, Platform.getProduct().getName(), e.getMessage());
 
    return IApplication.EXIT_OK;
}
 
...
```

