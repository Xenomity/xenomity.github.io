---
title: "Spring에서 Hibernate Validator 사용하기"
tags: [hibernate-validator, jsr-303, spring]
date: 2011-07-21T08:31:04+09:00
---

* 작업환경 : Spring 3.0.5, Hibernate Validator 4.2
  
Hibernate Validator는 JSR-303 Spec의 구현체로서 도메인 모델에서 @Annotation을 통한 필드값 검증을 가능하게 해준다. Controller에서의 Validation으로 지저분해지던 코드를 쉽게 도메인 오브젝트로 분리할 수 있으며, Spring의 MessageSource를 공유하여 손쉬운 에러메세지 관리도 가능하게 한다.  
  
- Maven pom.xml

```xml
<!-- Hibernate Validator -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>4.2.0.Final</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.5.11</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.5.11</version>
</dependency>
```
  
- Spring applicationContext.xml

```xml
<bean id="validator"
  class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
  <property name="validationMessageSource" ref="messageSource" />
</bean>
```
  
Hibernate Validator에서 사용 가능한 JSR-303 @Annotation 목록은 다음과 같다.  
  
#### `javax.validation.constraints.*`

| Annotation | Type | Validation |
|------------|------|------------|
| **@AssertFalse** | 모두 | 거짓여부 체크 |
| **@AssertTrue** | 모두 | 참여부 체크 |
| **@Length(min=, max=)** | 문자열 | 문자열 길이 체크 |
| **@Max(value=)** | 숫자 | 최대수 체크 |
| **@Min(value=)** | 숫자 | 최소수 체크 |
| **@NotNull** | 모두 | Null 체크 |
| **@Past** | 날짜 | 과거날짜 여부 체크 |
| **@Pattern(string="regexp", flag=)** | 문자열 | 문자열 정규표현식 체크 |
| **@Range(min=, max=)** | 숫자 | 숫자 범위 체크 |
| **@Size(min=, max=)** | 컬렉션류 (Array, Map 등등) | 컬렉션 크기 범위 체크 |

  
이 JSR-303 어노테이션 외에도 Hibernate Validator에서 자체 지원하는 추가 어노테이션들이 org.hibernate.validator.constraints 패키지에 정의되어 있다. 대충 눈에 띄는것들을 열거하자면,  
@NotEmpty, @Email, @URL, @CreditCardNumber 등등...  
  
웬만한 검증 패턴을 대부분 지원하기에... 노가다가 많이 줄어든다.;;  
다음은 @Valid Annotation을 이용한 간단한 적용 예제를 작성한 것이다.  
  
예1) Controller  
```java
@Controller
public class EmployeeController {
    @Autowired private Validator validator;
    ...
    
    @RequestMapping(value = "/employee", method = RequestMethod.POST)
    public String addEmployee(@Valid @ModelAttribute("employee") EmployeeForm employeeForm, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) { ... }
        ...
    }
}
```
  
예2) EmployeeForm.java  
```java
@SuppressWarnings("serial")
public class EmployeeForm extends Entity {
    @NotEmpty private String name;
    ...
}
```
  
예3) Spring MessageSource Properties File  
```
# Validation Message  
NotEmpty.employee.name=공백은 안되요~!
Min.employee.age={0} 이상만 가능합니다.
```

메세지의 사용 범위에 따라 프로퍼티의 키를 다양하게 정의할 수 있다. 기본적으로는 메세지 키는 _'어노테이션명.모델명.필드명'_이 된다. 이 룰을 따르기 싫다면 각각 어노테이션에서 직접 메세지 키를 정의하여 매핑할 수도 있다. 예를 들어 `@Email(message="{aaa.bbb}")`라면, `aaa.bbb`라는 프로퍼티 키의 메세지와 매핑된다.  
  
![hibernate validation](../assets/images/2011-07-21-201107210824.jpg)

