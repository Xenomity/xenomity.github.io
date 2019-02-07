---
title: "Spring Hadoop 1.0.0.M1 Milestone Released"
tags: [hadoop, spring]
date: 2012-03-05T10:33:41+09:00
---

얼마전 마일스톤 버전으로 릴리즈된 Spring Hadoop 프로젝트.  
개인적으로 많은 기대를 하고 있던 Spring 서브 프로젝트로서 현재일자 기준 최신버전은 1.0.0.M1이다.

Spring Hadoop은 Spring 환경에서의 Hadoop 기술을 기반으로 하는 어플리케이션을 개발할 수 있도록 하며, 독립형 어플리케이션이든 MapReduce 어플리케이션, 다양한 데이터 저장소로부터의 데이터와 상호 작용, HDFS의 복잡한 워크플로우 조정, Pig, Hive jobs 등에 Spring 철학에 따른 심플한 프로그래밍 모델을 제공한다.

다음은 SpringSource.org의 Spring Hadoop 샘플을 발췌한 내용이다.  
[http://blog.springsource.org/2012/02/29/introducing-spring-hadoop/](http://blog.springsource.org/2012/02/29/introducing-spring-hadoop/)  
  
  
**MapReduce Jobs**  
   
The Hello world for Hadoop is the word count example? a simple use-case that exposes the base Hadoop capabilities. When using Spring Hadoop, the word count example looks as follows:

| `<!-- configure Hadoop FS/job tracker using defaults -->` |

| `<``hdp:configuration` `/>` |

| |

| `<!-- define the job -->` |

| `<``hdp:job` `id``=``"word-count"` |

| ```    input-path``=``"/input/"` `output-path``=``"/ouput/"` |

| ```    mapper``=``"org.apache.hadoop.examples.WordCount.TokenizerMapper"` |

| ```    reducer``=``"org.apache.hadoop.examples.WordCount.IntSumReducer"``/>` |

| |

| `<!-- execute the job -->` |

| `<``bean` `id``=``"runner"` `class``=``"org.springframework.data.hadoop.mapreduce.JobRunner"` |

| ```    p:jobs-ref``=``"word-count"``/>` |

<font style="BACKGROUND-COLOR: #eeeeee">Notice how the creation and submission of the job configuration is handled by the IoC container. Whether the Hadoop configuration needs to be tweaked or the reducer needs extra parameters, all the configuration options are still available for you to configure. This allows you to start small and have the configuration grow alongside the app. The configuration can be as simple or advanced as the developer wants/needs it to be taking advantage of Spring container functionality such as property placeholders and environment support:</font>  

<font face="Courier New"><span style="FONT-FAMILY: Courier New">&lt;hdp:configuration</span></font> `resources``=``"classpath:/my-cluster-site.xml"``>` 

| ```    fs.default.name=${hd.fs}` |

| ```    hadoop.tmp.dir=file://${java.io.tmpdir}` |

| ```    electric=sea` |

| `</``hdp:configuration``>` |

| |

| `<``context:property-placeholder` `location``=``"classpath:hadoop.properties"` `/>` |

| |

| `<!-- populate Hadoop distributed cache -->` |

| `<``hdp:cache` `create-symlink``=``"true"``>` |

| ```    <``hdp:classpath` `value``=``"/cp/some-library.jar#library.jar"` `/>` |

| ```    <``hdp:cache` `value``=``"/cache/some-archive.tgz#main-archive"` `/>` |

| ```    <``hdp:cache` `value``=``"/cache/some-resource.res"` `/>` |

| `</``hdp:cache``>` |

<font style="BACKGROUND-COLOR: #eeeeee">(the word count example is part of the Spring Hadoop distribution? feel free to download it and experiment).<br>
 <br>
Spring Hadoop does not require one to rewrite your MapReduce job in Java, you can use non-Java streaming jobs seamlessly: they are just objects (or as Spring calls them beans) that are created, configured, wired and managed just like any other by the framework in a consistent, coherent manner. The developer can mix and match according to her preference and requirements without having to worry about integration issues.<br>

</font>
<font style="BACKGROUND-COLOR: #eeeeee">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:streaming</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"streaming-env"</span></code>
</td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml color1"><span style="FONT-FAMILY: Courier New">    input-path</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"/input/"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">output-path</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"/ouput/"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml color1"><span style="FONT-FAMILY: Courier New">    mapper</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"${path.cat}"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">reducer</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"${path.wc}"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:cmd-env</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">        EXAMPLE_DIR=/home/example/dictionaries/</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:cmd-env</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:streaming</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
<br>
</div></font>
<font style="BACKGROUND-COLOR: #eeeeee">Existing Hadoop Tool implementations are also supported; in fact rather than specifying custom Hadoop properties through the command line, one can simply inject it:<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- the tool automatically is injected with 'hadoop-configuration' --&gt;</span></code></td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:tool-runner</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"scalding"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">tool-class</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"com.twitter.scalding.Tool"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:arg</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">value</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"tutorial/Tutorial1"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:arg</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">value</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"--local"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:tool-runner</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
<br>
</div>
</div>The configuration above executes Tutorial1 of Twitter's Scalding (a Scala DSL on top of Cascading (see below) library. Note there is no dedicated support code in either Spring Hadoop or Scalding ? just the standard, Hadoop APIs are being used.<br>
 <br>
<br><strong><span style="FONT-SIZE: 12pt">Working with HBase/Hive/Pig</span><br>
</strong> <br>
Speaking of DSLs, it is quite common to use higher-level abstractions when interacting with Hadoop ? popular choices include HBase, Hive or Pig. Spring Hadoop provides integration for all of these, allowing easy configuration and consumption of these data sources inside a Spring app:<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- HBase configuration with nested properties --&gt;</span></code></td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:hbase-configuration</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">stop-proxy</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"false"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">delete-connection</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"true"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    foo=bar</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:hbase-configuration</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- create a Pig instance using custom properties</span></code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml comments"><span style="FONT-FAMILY: Courier New">and execute a script (using given arguments) at startup --&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:pig</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">properties-location</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"pig-dev.properties"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">script</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">location</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"org/company/pig/script.pig"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">        &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">arguments</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;electric=tears&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">arguments</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">script</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:pig</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
</div>Through Spring Hadoop, one not only gets a powerful IoC container but also access to Spring's portable service abstractions. Take the popular JdbcTemplate, one can use that on top of Hive's Jdbc client:<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- basic Hive driver bean --&gt;</span></code></td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">bean</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"hive-driver"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">class</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"org.apache.hadoop.hive.jdbc.HiveDriver"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- wrapping a basic datasource around the driver --&gt;</span></code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">bean</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"hive-ds"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml color1"><span style="FONT-FAMILY: Courier New">    class</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"org.springframework.jdbc.datasource.SimpleDriverDataSource"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml color1"><span style="FONT-FAMILY: Courier New">    c:driver-ref</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"hive-driver"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">c:url</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"${hive.url}"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content"><code class="xml comments"><span style="FONT-FAMILY: Courier New">&lt;!-- standard JdbcTemplate declaration --&gt;</span></code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">bean</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"template"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">class</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"org.springframework.jdbc.core.JdbcTemplate"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml color1"><span style="FONT-FAMILY: Courier New">    c:data-source-ref</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"hive-ds"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<br>
<br><strong><span style="FONT-SIZE: 12pt">Cascading</span></strong><br>
 <br>
Spring also supports a Java based, type-safe configuration model. One can use it as an alternative or complement to declarative XML configurations? such as with Cascading<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content"><code class="java color1"><span style="FONT-FAMILY: Courier New">@Configuration</span></code></td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="java keyword"><span style="FONT-FAMILY: Courier New">public</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java keyword"><span style="FONT-FAMILY: Courier New">class</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">CascadingConfig {</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java color1"><span style="FONT-FAMILY: Courier New">    @Value</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"${cascade.sec}"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">) </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">private</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">String sec;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java color1"><span style="FONT-FAMILY: Courier New">    @Bean</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java keyword"><span style="FONT-FAMILY: Courier New">public</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Pipe tsPipe() {</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java plain"><span style="FONT-FAMILY: Courier New">        DateParser dateParser = </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">DateParser(</span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Fields(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"ts"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">),</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java string"><span style="FONT-FAMILY: Courier New">                "dd/MMM/yyyy:HH:mm:ss Z"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">);</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">        return</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Each(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"arrival rate"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">, </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Fields(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"time"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">), dateParser);</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java plain"><span style="FONT-FAMILY: Courier New">    }</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java color1"><span style="FONT-FAMILY: Courier New">    @Bean</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java keyword"><span style="FONT-FAMILY: Courier New">public</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Pipe tsCountPipe() {</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java plain"><span style="FONT-FAMILY: Courier New">        Pipe tsCountPipe = </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Pipe(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"tsCount"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">, tsPipe());</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java plain"><span style="FONT-FAMILY: Courier New">        tsCountPipe = </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">GroupBy(tsCountPipe, </span></code><code class="java keyword"><span style="FONT-FAMILY: Courier New">new</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="java plain"><span style="FONT-FAMILY: Courier New">Fields(</span></code><code class="java string"><span style="FONT-FAMILY: Courier New">"ts"</span></code><code class="java plain"><span style="FONT-FAMILY: Courier New">));</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="java plain"><span style="FONT-FAMILY: Courier New">    }</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content"><code class="java plain"><span style="FONT-FAMILY: Courier New">}</span></code></td>
</tr>
</tbody>
</table>
<br>
</div>
</div>
<br>
</font>  

| `<!-- code configuration class -->` |

| `<``bean` `class``=``"org.springframework.data.hadoop.cascading.CascadingConfig "``/>` |

| |

| `<``bean` `id``=``"cascade"` |

| ```    class``=``"org.springframework.data.hadoop.cascading.HadoopFlowFactoryBean"` |

| ```    p:configuration-ref``=``"hadoop-configuration"` `p:tail-ref``=``"tsCountPipe"` `/>` |

<font style="BACKGROUND-COLOR: #eeeeee">The example above mixes both programmatic and declarative configurations: the former to create the individual Cascading pipes and the former to wire them together into a flow.<br>
 <br>
<br><strong><span style="FONT-SIZE: 12pt">Using Spring's portable service abstractions</span><br>
</strong> <br>
Or use Spring's excellent task/scheduling support to submit jobs at certain times:<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">task:scheduler</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">id</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"myScheduler"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">pool-size</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"10"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
<div class="line alt2"> </div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">task:scheduled-tasks</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">scheduler</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"myScheduler"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml comments"><span style="FONT-FAMILY: Courier New">    &lt;!-- run once a day, at midnight --&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    &lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">task:scheduled</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">ref</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"word-count-job"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">method</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"submit"</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">cron</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"0 0 * * * "</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">/&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">task:scheduled-tasks</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
</div>The configuration above uses a simple JDK Executor instance ? excellent for POC development. One can easily replace it (a one-liner) in production with a more comprehensive solution such as dedicated scheduler or a WorkManager implementation ? another example of Spring's powerful service abstractions.<br>
 <br>
<br><strong><span style="FONT-SIZE: 12pt">HDFS/Scripting</span></strong><br>
 <br>
A common task when interacting with HDFS is preparing the file-system, such as cleaning the output directory to avoid overriding data or moving all input files under the same name scheme or folder. Spring Hadoop addresses the issue by fully embracing Hadoop's fs commands, such as FS Shell and DistCp and exposing them as proper Java APIs. Mix that along with JVM scripting (whether it is Groovy, JRuby or Rhino/JavaScript) to form a powerful combination:<br>

<div style="BORDER-BOTTOM: #c1c1c1 1px solid; BORDER-LEFT: #c1c1c1 1px solid; PADDING-BOTTOM: 10px; BACKGROUND-COLOR: #eeeeee; PADDING-LEFT: 10px; PADDING-RIGHT: 10px; BORDER-TOP: #c1c1c1 1px solid; BORDER-RIGHT: #c1c1c1 1px solid; PADDING-TOP: 10px" class="txc-textbox">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:script</span></code><span style="FONT-FAMILY: Courier New"> </span><code class="xml color1"><span style="FONT-FAMILY: Courier New">language</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">=</span></code><code class="xml string"><span style="FONT-FAMILY: Courier New">"groovy"</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    inputPath = "/user/gutenberg/input/word/"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    outputPath = "/user/gutenberg/output/word/"</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    if (fsh.test(inputPath)) {</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">        fsh.rmr(inputPath)</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    }</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    if (fsh.test(outputPath)) {</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">        fsh.rmr(outputPath)</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    }</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="content">
<code class="spaces"></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">    fs.copyFromLocalFile("data/input.txt", inputPath)</span></code>
</td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="content">
<code class="xml plain"><span style="FONT-FAMILY: Courier New">&lt;/</span></code><code class="xml keyword"><span style="FONT-FAMILY: Courier New">hdp:script</span></code><code class="xml plain"><span style="FONT-FAMILY: Courier New">&gt;</span></code>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<br>
</font>