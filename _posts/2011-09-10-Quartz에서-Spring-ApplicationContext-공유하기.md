---
title: "Quartz에서 Spring ApplicationContext 공유하기"
tags: [quartz, spring]
date: 2011-09-10T17:19:51+09:00
---

Quartz의 Life-cycle이 Spring과 별개 level에서 동작하는 이유로, @Resource나 @Autowired같은 Annotation 기반의 Dependency Injection을 사용하기에는 무리가 있다. 그래서 보통 quartz 설정에서 직접 Job의 setter에 bean을 주입하는 형태가 대부분이라(jobDataAsMap...) interface가 까다로워지는 단점이 생긴다.  
어떻게 Quartz를 유연하게 확장할 수 있을까 하는 생각을 하다가, 직접 JobDetailBean을 뜯어보니 다음과 같은 메서드들이 존재하더라..
  
참고로 현재 환경은 Spring 3.0.5, Quartz 1.8.5 버전이다.  
  
**예 1) org.springframework.scheduling.quartz.JobDetailBean.java**
```java
public void setApplicationContext(ApplicationContext applicationContext) {
    this.applicationContext = applicationContext;
}
 
...
 
public void setApplicationContextJobDataKey(String applicationContextJobDataKey) {
    this.applicationContextJobDataKey = applicationContextJobDataKey;
}
 
public void afterPropertiesSet() {
    if (getName() == null) {
        setName(this.beanName);
    }
    if (getGroup() == null) {
        setGroup(Scheduler.DEFAULT_GROUP);
    }
    if (this.applicationContextJobDataKey != null) {
        if (this.applicationContext == null) {
            throw new IllegalStateException(
     "JobDetailBean needs to be set up in an ApplicationContext " +
     "to be able to handle an 'applicationContextJobDataKey'");
        }
        getJobDataMap().put(this.applicationContextJobDataKey, this.applicationContext);
    }
}
```
  
위에서 보듯, JobDetailBean을 빈으로 등록시에 setApplicationContextJobDataKey에 applicationContext를 넘겨주면 Quartz에서는 Spring의 applicationContext에 등록된 빈들이 사용 가능한 상태가 된다.  
  
**예 2) quartz.xml**  
```xml
<bean id="employeeCounterTrigger" class="org.springframework.scheduling.quartz.CronTriggerBean">
    <property name="jobDetail">
        <bean id="employeeCounterJob" class="org.springframework.scheduling.quartz.JobDetailBean">
            <property name="jobClass"
     value="com.xenomity.employee.batch.EmployeeCounterJob" />
            <property name="applicationContextJobDataKey" value="applicationContext" />
        </bean>
    </property>
    <property name="cronExpression" value="0/10 * * * * ?" />
</bean>
```
  
실제 생성한 Job에서 setter를 구현할 필요도 없고, jobDataAsMap을 통한 레퍼런스 주입도 필요없어지므로 훨씬 깔끔해졌다. 그럼 실제 Spring의 Bean을 어떻게 가져올것인가에 대한 인터페이스만 추상화해주면 좀 더 나아 보일듯하다.  
  
**예 3) AbstractJob.java**  
```java
/**
 * Abstract Job
 *
 * @author Xenomity™ a.k.a P-slinc' (이승한)
 */
public abstract class AbstractJob extends QuartzJobBean {
    private ApplicationContext ctx;
 
    ...
 
    @Override
    protected void executeInternal(JobExecutionContext context)
            throws JobExecutionException {
        ctx = (ApplicationContext) context.getJobDetail().getJobDataMap().get("applicationContext");
   
        // execute batch
        executeJob(context);
    }
  
    /**
     * Get Bean
     *
     * @param beanId Spring Bean ID
     * @return Bean Instance
     */
    protected Object getBean(String beanId) {
        return ctx.getBean(beanId);
    }
}
```
  
Spring ApplicationContext를 이용해 Bean을 획득하는 getBean() 메서드까지 만들었다. 개발자들에게는 실제 Job을 실행하고 스케줄링하는 executeInternal 메서드 대신, 내부의 템플릿 메서드인 executeJob을 오버라이딩하도록 강제시킨다.  
  
**예 4)TestJob.java**  
```java
public class TestJob extends AbstractJob {
  
    @Override
    protected void executeJob(JobExecutionContext context)
            throws JobExecutionException {
        TestService testService = (TestService) super.getBean("TestServiceImpl");
   
        if (logger.isInfoEnabled()) {
            logger.info("MAX ID: " + employeeService.getMaxID());
        }
    }
}
```
  
  
이상 Quartz 삽질중에 날림글~!!...  
  
