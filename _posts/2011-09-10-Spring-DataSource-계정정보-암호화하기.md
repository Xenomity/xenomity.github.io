---
title: "Spring DataSource 계정정보 암호화하기"
tags: [crypto, dbcp, spring]
published: 2011-09-10T14:20:00+09:00
---

jasypt (Java Simplified Encryption)을 사용하면 손쉽게 properties 파일 내부의 값을 암호화가 가능하다. 그러나 Spring의 applicationContext 값을 암호화하려면, 값을 properties로 분리하고 PropertyPlaceHolder를 통해 접근해야 하는데 이또한 귀차니즘;;;이라...  
  
jasypt에 대한 자세한 내용은 다음을 참고 : [http://www.jasypt.org/](http://www.jasypt.org/)  
  
  
요즘 만사가 귀찮은 관계로..; 그냥 사용하고 있는 DataSource를 직접 확장하여 구현하기로 했다. ㅡ,.ㅡa  
테스트로 사용하는 DataSource는 Apache DBCP의 org.apache.commons.dbcp.BasicDataSource이며, 이를 확장하여 실제 암호화가 필요한 메서드만 오버라이딩하였다. (url, username, password)  
  
예 1) test.CryptoDataSource.java
```java
/**
 * Crypto DataSource
 *
 * @author Xenomity™ a.k.a P-slinc' (이승한)
 */
public class CryptoDataSource extends BasicDataSource {
 
    @Override
    public synchronized void setUrl(String url) {
        super.setUrl(AES.decrypt(url));
    }
  
    @Override
    public void setUsername(String username) {
        super.setUsername(AES.decrypt(username));
    }
  
    @Override
    public void setPassword(String password) {
        super.setPassword(AES.decrypt(password));
    }
  
}
```
  
테스트에 사용한 암복호화 방식은 AES를 사용하였으며, 실제 applicationContext의 DataSource에 설정되는 필드들 중, setUrl, setUsername, setPassword 세가지 메서드를 오버라이딩하고 내부에서는 암호화된 값을 복호화하는 단계를 추가시켰다.  
  
예 2) applicationContext.xml
```xml
<bean id="dataSource" class="test.CryptoDataSource" destroy-method="close">
    <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
    <property name="url" value="bbIo1Z6IBh6zwxHLgjvZnV12=" />
    <property name="username" value="aUFpVIplNtXUNok8tHy3Zw==" />
    <property name="password" value="VtMMhqMrd+X5LGUIFDkq0A==" />
    <property name="initialSize" value="10" />
    <property name="maxActive" value="50" />
</bean>
```
  
위의 url, username, password의 값은 AES를 통해 암호화한 값을 넣어준 것이다. CryptoDataSource가 생성되고 DI에 의해 암호화된 값을 주입받으면, 실제 오버라이딩된 메서드에서는 복호화를 선처리하고 데이터를 세팅한다.  
  
