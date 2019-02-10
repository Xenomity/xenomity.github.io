---
title: "E-Mail Domain 유효성 검사"
tags: java
date: 2010-12-27T16:30:14+09:00
---

가장 확실한 방법은 Java Mail이나 Socket 등을 통해 해당 메일 서버로 직접 연결하여 응답코드를 받아오는 것이지만, 테스트 결과 여러가지 문제점 및 복잡성 증가가 있다.  
연결을 거부하는 곳도 있고, SSL 연결 여부, 포트 등...  
대신 해당 메일 서버에 해당 계정이 사용가능한지 여부도 확인할 수 있는 장점도 있다.  
  
귀차니즘으로(-0-), 해당 메일 도메인만 nslookup을 통해 유효성 검증을 하는 간단한 코드를 작성해 봤다.   
  
nslookup mx record 조회로 mail exchanger를 통하는지 여부로 검사하는 방식.  
**> nslookup -type=MX {domain명}**
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
 
public class ValidateMailServer {
 
    /** NSLOOKUP Command */
    public static final String LOOKUP_COMMAND = "nslookup -type=MX ";
 
    /** Valid Prefix */
    public static final String VALID_PREFIX = "mail exchanger";
 
    /** Mail Address Delimiter */
    public static final String MAIL_DELIMITER = "@";
 
    /**
     * Get Output Data
     *
     * @param command Native Command
     * @return Output Data
     * @throws IOException
     */
    public String getOutputData(String command) throws IOException {
        Process process = null;
        BufferedReader reader = null;
        StringBuffer buffer = new StringBuffer();
 
        try {
            // Execute Native Command
            process = Runtime.getRuntime().exec(command);
            reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
            String temp = null;
 
            while ((temp = reader.readLine()) != null) {
                buffer.append(temp);
                buffer.append("\n");
            }
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
 
            if (process != null) {
                process.destroy();
            }
        }
 
        return buffer.toString();
    }
 
    /**
     * Validate E-Mail Server
     *
     * @param emailAddress E-Mail Address
     * @return is-valid
     * @throws IOException
     */
    public boolean isValid(String emailAddress) throws IOException {
        boolean result = false;
 
        if (emailAddress != null && emailAddress.indexOf(MAIL_DELIMITER) > -1) {
            String domain = emailAddress.substring(emailAddress.indexOf(MAIL_DELIMITER) + 1);
            String outputData = getOutputData(LOOKUP_COMMAND + domain);
 
            if (outputData.contains(VALID_PREFIX)) {
                result = true;
            }
        }
 
        return result;
    }
 
    public static void main(String[] args) throws IOException {
        ValidateMailServer vms = new ValidateMailServer();
 
        System.out.println(vms.isValid(MyMailAddress));
    }
}
```
