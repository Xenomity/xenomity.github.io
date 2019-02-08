---
title: "Maven PMD Reporting Plug-in"
tags: [hudson, maven, pmd]
date: 2011-03-20T19:04:00+09:00
---

Maven을 통한 빌드시 PMD Code Inspection의 결과물을 내보낼 수 있게 한다. 'pmd:pmd' Goal을 실행하면 정해진 경로로 xml 결과물이 생성되며, Hudson(Jenkins)과 같은 CI의 plug-in과 조합하여 상당한 수준의 관리 통합도 가능하다.  
또한 'pmd:cpd' Goal을 통해 Copy & Paset Detector(CPD)도 이용할 수 있다.  
  
  
**1. Maven POM**
```xml
    <!-- Reporting -->
    <reporting>
        <plugins>
        ...
            <plugin>
                <groupid>org.apache.maven.plugins</groupid>
                <artifactid>maven-pmd-plugin</artifactid>
                <version>2.4</version>
                <configuration>
                    <rulesets>
                        <ruleset>rulesets/unusedcode.xml</ruleset>
                        <ruleset>rulesets/coupling.xml</ruleset>
                        <ruleset>rulesets/clone.xml</ruleset>
                        <ruleset>rulesets/codesize.xml</ruleset>
                        <ruleset>rulesets/braces.xml</ruleset>
                        <ruleset>rulesets/naming.xml</ruleset>
                        <ruleset>rulesets/optimizations.xml</ruleset>
                        <ruleset>rulesets/imports.xml</ruleset>
                        <ruleset>rulesets/logging-java.xml</ruleset>
                        <ruleset>rulesets/strings.xml</ruleset>
                    </rulesets>
                    <sourceencoding>utf-8</sourceencoding>
                    <targetjdk>1.6</targetjdk>
                    <minimumtokens>10</minimumtokens>
                    <excludes>
                        <exclude> **/com/xenomity/common/** /*.java</exclude>
                    </excludes>
                </configuration>
            </plugin>
            ...
        </plugins>
    </reporting>
```
  
  
**2. Rulesets**  
`maven-pmd-plugin.jar` 내부에는 `/rulesets` 경로에 각 카테고리에 해당하는 ruleset XML 파일들이 정의되어 있다. 제공 룰셋들이 너무 엄격하거나 loose하다면, 커스터마이징도 물론 가능하다.
자세한 내용은 [http://pmd.sourceforge.net/"](http://pmd.sourceforge.net/)을 참고한다.

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Android_Rules">Android Rules</a>: These rules deal with the Android SDK, mostly related to best practices. To get better results, make sure that the auxclasspath is defined for type resolution to work. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Basic_JSF_rules">Basic JSF rules</a>: Rules concerning basic JSF guidelines. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Basic_JSP_rules">Basic JSP rules</a>: Rules concerning basic JSP guidelines.</li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Basic_Rules">Basic Rules</a>: The Basic Ruleset contains a collection of good practices which everyone should follow. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Braces_Rules">Braces Rules</a>: The Braces Ruleset contains a collection of braces rules. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Clone_Implementation_Rules">Clone Implementation Rules</a>: The Clone Implementation ruleset contains a collection of rules that find questionable usages of the clone() method. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Code_Size_Rules">Code Size Rules</a>: The Code Size Ruleset contains a collection of rules that find code size related problems. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Controversial_Rules">Controversial Rules</a>: The Controversial Ruleset contains rules that, for whatever reason, are considered controversial. They are separated out here to allow people to include as they see fit via custom rulesets. This ruleset was initially created in response to discussions over UnnecessaryConstructorRule which Tom likes but most people really dislike :-) </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Coupling_Rules">Coupling Rules</a>: These are rules which find instances of high or inappropriate coupling between objects and packages. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Design_Rules">Design Rules</a>: The Design Ruleset contains a collection of rules that find questionable designs. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Finalizer_Rules">Finalizer Rules</a>: These rules deal with different problems that can occur with finalizers. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Import_Statement_Rules">Import Statement Rules</a>: These rules deal with different problems that can occur with a class' import statements. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#J2EE_Rules">J2EE Rules</a>: These are rules for J2EE </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#JavaBean_Rules">JavaBean Rules</a>: The JavaBeans Ruleset catches instances of bean rules not being followed. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#JUnit_Rules">JUnit Rules</a>: These rules deal with different problems that can occur with JUnit tests. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Jakarta_Commons_Logging_Rules">Jakarta Commons Logging Rules</a>: The Jakarta Commons Logging ruleset contains a collection of rules that find questionable usages of that framework. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Java_Logging_Rules">Java Logging Rules</a>: The Java Logging ruleset contains a collection of rules that find questionable usages of the logger. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Migration_Rules">Migration Rules</a>: Contains rules about migrating from one JDK version to another. Don't use these rules directly, rather, use a wrapper ruleset such as migrating_to_13.xml. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Migration13">Migration13</a>: Contains rules for migrating to JDK 1.3 </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Migration14">Migration14</a>: Contains rules for migrating to JDK 1.4 </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Migration15">Migration15</a>: Contains rules for migrating to JDK 1.5 </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#MigratingToJava4">MigratingToJava4</a>: Contains rules for migrating to JDK 1.5 </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Naming_Rules">Naming Rules</a>: The Naming Ruleset contains a collection of rules about names - too long, too short, and so forth. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Optimization_Rules">Optimization Rules</a>: These rules deal with different optimizations that generally apply to performance best practices. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Strict_Exception_Rules">Strict Exception Rules</a>: These rules provide some strict guidelines about throwing and catching exceptions. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#String_and_StringBuffer_Rules">String and StringBuffer Rules</a>: These rules deal with different problems that can occur with manipulation of the class String or StringBuffer. </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Security_Code_Guidelines">Security Code Guidelines</a>: These rules check the security guidelines from Sun, published at http://java.sun.com/security/seccodeguide.html#gcg </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Type_Resolution_Rules">Type Resolution Rules</a>: These are rules which resolve java Class files for comparisson, as opposed to a String </li>
<li>
<a href="http://pmd.sourceforge.net/rules/index.html#Unused_Code_Rules">Unused Code Rules</a>: The Unused Code Ruleset contains a collection of rules that find unused code. <br>
</li>
</div>

**3. Maven Goal List**
<table style="BORDER-COLLAPSE: collapse" cellspacing="1" cellpadding="1" width="580" bgcolor="#ffffff">
<tbody>
<tr>
<td style="BORDER-BOTTOM: #dadada 1px solid; BORDER-LEFT: #dadada 1px solid; BORDER-TOP: #dadada 1px solid; BORDER-RIGHT: #dadada 1px solid" width="50%"><strong>pmd:pmd</strong></td>
<td style="BORDER-BOTTOM: #dadada 1px solid; BORDER-LEFT: #dadada 1px solid; BORDER-TOP: #dadada 1px solid; BORDER-RIGHT: #dadada 1px solid" width="50%">PMD Report XML, CSV or TXT 생성 </td>
</tr>
<tr>
<td style="BORDER-BOTTOM: #dadada 1px solid; BORDER-LEFT: #dadada 1px solid; BORDER-TOP: #dadada 1px solid; BORDER-RIGHT: #dadada 1px solid" width="50%"><strong>pmd:cpd</strong></td>
<td style="BORDER-BOTTOM: #dadada 1px solid; BORDER-LEFT: #dadada 1px solid; BORDER-TOP: #dadada 1px solid; BORDER-RIGHT: #dadada 1px solid" width="50%">CPD Report XML, CSV or TXT 생성</td>
</tr>
</tbody>
</table>

**4. Hudson CI**
Hudson 2.1.0 기준으로 PMD/CPD의 결과를 보려면 우선 다음과 같은 plug-in들이 설치되어 있어야 한다.

![hudson plugins](/assets/image/2011-03-20-201108201745.jpg)

그리고 원하는 Job의 Configure -> Post-Build Action에 다음 사항을을 확인한다.  

![post build](/assets/image/2011-03-20-201108201908.jpg)
  
Hudson의 해당 Job workspace가 PMD Reporting의 디폴트 경로가 되며, 별다른 설정을 하지 않으면 plug-in에서는 workspace/reports 경로에 결과물을 생성시킨다. 디폴트 경로르 사용하는 경우, 위 설정에서 별다른 result XML을 설정해줄 필요는 없다.  
  
성공적으로 빌드가 끝나면 hudson의 좌측 메뉴에 'PMD Warnings'과 'Duplicate Code'라는 메뉴가 추가된 것을 확인할 수 있다.  

![PMD Result 1](/assets/image/2011-03-20-201108201923.jpg)

![PMD Result 2](/assets/image/2011-03-20-201108201920.jpg)
  

 

  
  
