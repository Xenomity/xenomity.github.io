---
title: "Spring 3 RESTful style MVC 예"
tags: spring
date: 2011-07-20T05:48:00+09:00
---

Spring 3 @MVC를 통하여 RESTful한 스타일의 서비스를 쉽게 구현할 수 있다. Spring Form 태그를 통한 GET, PUT, DELETE, POST HTTP 요청으로 동일한 자원요청에 대한 CRUD를 다양하게 처리할 수 있으며, 별도로 `ContentNegotiatingViewResolver`나 `HttpMessageConverter`와 함께 dynamic한 전송 포맷에 대한 핸들링도 가능하다.  
  
참고로 HTML Form으로는 GET, POST 방식의 요청만 가능한 관계로, 실제 Spring에서는 GET, POST 이외의 요청은 hidden 값으로 `_method` 파라메터를 넘겨주는 트릭을 이용한다. Spring Form 태그를 통하여 `<form method="PUT">`의 경우, 실제 변환된 HTML 코드를 보면 폼 내부의 `<input type="hidden" name="_method" value="PUT" />`으로 변환되는걸 확인할 수 있다.
```html
<!-- Spring Form -->
<form:form method="PUT" action="gg">
</form:form>
 
<!-- HTML Form -->
<form method="post" action="gg">
    <input type="hidden" name="_method" value="PUT" />
</form>
```
  
그리하여 서블릿단에는 이 정보를 확인하고 실제 해당하는 HTTP 요청으로 변경하여주는 HiddenHttpMethodFilter 필터가 추가로 등록되어야 한다.  
  
예) web.xml  
```xml
<!-- HTTP Method Filter -->
<filter>
    <filter-name>httpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>httpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
  
다음은 동일한 URI 요청으로 각기 다른 CRUD를 행하는 예를 간단히 작성해 보았다.
  
예1) jsp  
```jsp
<!-- ID가 1인 직원정보 조회 -->
<form:form method="GET" action="/employee/1">
</form:form>
 
<!-- ID가 1인 직원정보 수정 -->
<form:form method="PUT" action="/employee/1">
</form:form>
 
<!-- ID가 1인 직원정보 삭제 -->
<form:form method="DELETE" action="/employee/1">
</form:form>
 
<!-- 직원정보 등록 -->
<form:form method="POST" action="/employee">
</form:form>
```
  
예2) Controller 
```java
/**
 * Employee Controller
 *
 * @author Xenomity™ a.k.a P-slinc' (이승한)
 */
@Controller
public class EmployeeController {
  
 ...
  
 /**
  * 직원정보 조회
  */
 @RequestMapping(value = "/employee/{id}", method = RequestMethod.GET)
 public String getEmployee(@PathVariable int id, Model model) {
  ...
 }
  
 /**
  * 직원정보 등록
  */
 @RequestMapping(value = "/employee", method = RequestMethod.POST)
 public String addEmployee(
   @ModelAttribute("employee") EmployeeForm employeeForm,
   BindingResult bindingResult) {
  ...
 }
  
 /**
  * 직원정보 삭제
  */
 @RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
 public String removeEmployee(@PathVariable int id) {
  ...
 }
  
 /**
  * 직원정보 수정
  */
 @RequestMapping(value = "/employee/{id}", method = RequestMethod.PUT)
 public String setEmployee(
   @PathVariable int id,
   @ModelAttribute("employee") EmployeeForm employeeForm,
   BindingResult bindingResult) {
  ...
 }
  
}
```
