---
title: "Java 성능 튜닝 -3- Overhead의 예"
tags: [java]
date: 2010-03-09T10:18:00+09:00
---

동일한 로직에 대한 성능차가 얼마나 큰 Overhead를 일으키는지에 대한 예를 작성해 보았다.  
다음은 간단한 Echo의 예이다.  
  
예 1)
```
public class Echo1 {
 
    ...
 
    // Get Server Status
    private boolean isRunning() { ... }
 
    // Write Echo
    private void echo(Client client, String echoMeesage) {
        if (isRunning()) {
            client.write(echoMessage);
        }
    }
 
    /**
     * Run
     */
    public void run() {
        for (int i = 0; i < clientPool.getClientSize(); i++) {
            echo(clientPool.getClient(i), "echo " + i);
        }
    }
}
```
  
예 2)  
```
public class Echo2 {
 
    ...
 
    public static final boolean isRunning = ...;
 
    // Write Echo
    private void echo(Client client, String echoMeesage) {
        client.write(echoMessage);
    }
 
    /**
     * Run
     */
    public void run() {
        int clientSize = clientPool.getClientSize();
 
        for (int i = 0; i < clientSize; i++) {
            if (isRunning) {           
                echo(clientPool.getClient(i), "echo " + i);
            }
        }
    }
}
```
  
결과)  
```
Echo 1 : 9,145ms  
Echo 2 : 224ms  
```
  
두 예제 모두 클라이언트들에게 단순한 메세지를 보내는 동일한 로직이지만, 상당히 큰 수행시간의 차이가 난다. 하나씩 짚어보면,  
  
1. Echo1에서는 서버의 상태를 가져오는 isRunning 메서드가 정의되어 있고, Echo2에서는 상수화시켜 놓았다. 변수값의 취득이 메서드 호출의 결과값 취득보다 엄청난 차이로 빠르다는 것은 당연~  
  
2. Echo1에서는 echo 메서드 내부에서 서버의 상태를 체크하지만, Echo2에서는 Run 메서드에서 서버의 상태를 체크한다. 동작은 동일하지만 echo 메서드가 호출되느냐 아니냐가 관건인데, run 메서드에서 echo 메서드를 호출하기 위해 Echo1의 경우는 불필요한 String 객체가 생성되어 파라메터로 넘어간다. 결국 쓰레기 객체의 반복적 생성과 가비지 컬렉션의 부하가 극심한 오버헤드를 일으키게 된다. 실제 테스트시 가장 큰 성능차이를 보인 부분이다.  
  
3. run 메서드에서 for문을 위한 max값을 가져오는 부분이 Echo1에서는 악성 코드로 되어있다. 실제 for문에서 루프를 돌 때마다 getClientSize()가 호출되면서 값이 클수록 오버헤드를 일으킨다.
