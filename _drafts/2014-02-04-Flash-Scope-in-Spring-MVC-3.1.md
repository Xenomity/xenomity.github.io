---
title: "Flash Scope in Spring MVC 3.1"
tags: spring
date: 2014-02-04T17:58:49+09:00
---

Spring MVC 3.1에 추가된 Flash Attribute Scope는, Redirect View Handling 전에 HTTP 요청 데이터를 임시 보관하였다가 재전송하기 위한 Attribute Scope이다. 이런 상황에서, 보통 개발자들은 다른 방법을 강구하거나 HTTP Session을 통해 전송 데이터를 임시 보관하는 등의 방법을 사용하지만, redirect 후에 임시 데이터 수거 코드가 적절히 구현되지 않으면, 불필요한 Session store 낭비 현상이 발생되기도 한다. 이 문제를 편하게 해결하기 위해, Spring MVC는 'RedirectAttributes' 클래스의 flash attributes 또는 FlashMap 클래스를 통해 간편하게 세션에 데이터를 저장하고 redirect 후 데이터를 제거하는 일련의 과정들을 단순하게 제공한다.

| ** Class** | ** Operation** |
|-|-|
| RedirectAttributes | addFlashAttribute(...) |
| FlashMap | RequestContextUtils.getOutputFlashMap(request) |

- 표 1. Flash scope 지원 클래스

다음은 flash scope을 통한 redirect시, 데이터 전달과 획득에 대한 간략한 예제이다.
```java
    @Controller public class FlashScopeTestController { @RequestMapping(value = "/test") public String getResult(RedirectAttributes redirectAttributes) { // Generate Temporary Data UserDetails userDetails = new UserDetails(); userDetails.setUserName("Xenomity"); // Add Data to Flash Scope redirectAttributes.addFlashAttribute(userDetails); // FlashMap flashMap = RequestContextUtils.getOutputFlashMap(request); // Redirect return "redirect:/redirected"; } @RequestMapping(value = "/redirected") public String getResultRedirect(@ModelAttribute UserDetails userDetails) { return "testPage"; } }
```
- 예 1. TestController.java

```jsp
    \<html\> \<body\>     \<h1\>Hi~! ${userDetails.userName}.\</h1\> \</body\> \</html\>
```
- 예 2. testPage.jsp

```
GET /test HTTP 1.1

Hi~ Xenomity.
```
- 예 3. 테스트 결과

