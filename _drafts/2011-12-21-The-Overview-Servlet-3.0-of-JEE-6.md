---
title: "The Overview Servlet 3.0 of JEE 6"
tags: [java, jee]
date: 2011-12-21T13:58:22+09:00
---

Servlet 3.0 (JSR-315) API는 개발 편의성의 증대라는 목표로 Annotation을 통한 선언적 프로그래밍, 하위 호환성 문제가 없는 type-safety한 Generics, 손쉬운 Web Application 설정 등을 지원한다. 역시나 눈에 띄는 부분은 Annotation을 통한 meta-data 설정으로, Spring이나 Tapestry 등의 프레임워크에서 볼 수 있는 손쉬운 Annotation 기반 설정 기능을 제공한다. 덕분에 web.xml 배포기술자 파일이 없는 Web Application의 개발이 가능하게 한다.  
  
아래부터는 Servlet 3.0 Specification Final Release Document의 내용을 참고하여 간략히 정리해 보았다.

#### **1. Annotations**
 **1.1. @WebServlet**  

    @WebServlet(”/test”) public class TestServlet extends HttpServlet { ... } @WebServlet(name=”testServlet”, urlPatterns={"/test", "/test.do"}) public class TestServlet extends HttpServlet { ... } @WebServlet(name="testServlet", urlPatterns={"/test"}, initParams={ @WebInitParam(name="author", value="xenomity") }) public class TestServlet extends HttpServlet { .... } @WebServlet(name="testServlet", asyncSupported=false) public class TestServlet extends HttpServlet { .... }

  
**1.2. @WebListener**  

    @WebListener public class TestListener implements ServletContextListener { public void contextInitialized(ServletContextEvent sce) { ServletContext ctx = sce.getServletContext(); ctx.addServlet("testServlet", "Test Servlet", "com.xenomity.servlet.TestServlet", null, -1); ctx.addServletMapping("testServlet", new String[] { "/test/\*.do" }); } }

  
**1.3. @WebFilter** 

    @WebFilter(“/test”) public class TestFilter implements Filter { ... }

  
**1.4. @MultipartConfig**  
@MultipartConfig Annotation을 선언함으로서 HttpServletRequest의 getPart(...)를 통해 Part 객체를 획득할 수 있다. Part 객체는 Spring Framework의 MultipartFile과 그 용도가 비슷한 Wrapper Object이다.  
  

    @MultipartConfig(location="/tmpUpload") public class UploadServlet extends HttpServlet { public void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException { req.getPart("thumbFile").write("/upload/thumb.jpg"); } } @MultipartConfig(location="/tmpUpload", fileSizeThreshold=1024\*1024, maxFileSize=1024\*1024, maxRequestSize=1024\*1024\*5\*5) public class UploadServlet extends HttpServlet { ... }

@MultipartConfig의 'location' 속성값은 임시 업로드 경로를 지정하게 되며, Part 객체를 통해 write() 메서드가 호출되기 전까지는 임시 업로드 경로에 '~~~.tmp'라는 임시파일로 남아있게 된다.  
  
  

#### **2. Dynamic Registration of Servlets and Filters**
코드상에서 동적으로 서블릿 및 필터 클래스를 등록, 매핑 혹은 해제할 수 있다.  

    ServletRegistration.Dynamic dynamic = servletContext.addServlet("testServlet", "com.xenomity.servlet.TestServlet"); dynamic.addMapping("/test.do"); dynamic.setAsyncSupported(true); ServletRegistration declared = servletContext.getServletRegistration("testServlet"); declared.addMapping("/testServlet"); declared.setInitParameter("param", "value");

  
  

#### **3. Pluggability**
Annotation을 통한 서블릿, 필터 등의 정의 또는 web-fragment.xml을 통하여 배포기술자의 모듈화가 가능하다.  
  
  

#### **4. Asynchronous Support**

[##\_1C|cfile5.uf.1331A13E4EF417F91AFCBF.PNG|width="590" height="368" filename="201112231455.PNG" filemime="image/jpeg"|\_##]

  

- web.xml  
**\<async-supported\>true\</async-supported\>**  
  
- @WebServlet  
**@WebServlet(asyncSupported=true)**  
  
- Programmatic  
**registration.setAsyncSupported(boolean)**  

  
예 1) sample code  

    @WebServlet(urlPatterns={"/foo"}, asyncSupported=true) public class MyServlet extends HttpServlet { public void doGet(HttpServletRequest req, HttpServletResponse res) { ... AsyncContext aCtx = request.startAsync(req, res); ThreadPoolExecutor executor = new ScheduledThreadPoolExecutor(10); executor.execute(new AsyncWebService(aCtx)); } } public class AsyncWebService implements Runnable { AsyncContext ctx; public AsyncWebService(AsyncContext ctx) { this.ctx = ctx; } public void run() { // Invoke web service and save result in request attribute // Dispatch the request to render the result to a JSP. ctx.dispatch("/render.jsp"); } }

  
  

#### **5. Security**
Spring Security처럼 디테일한 수준은 아니지만, Security Annotations로 요청별 권한 인증을 어느정도 제어할 수 있다.  

|   **@RolesAllowed** |  auth-constraint with roles |
|   **@DenyAll** |  Empty auth-constraint |
|   **@PermitAll** |  No auth-constraint |
|   **@TransportProtected** |  user-data-constraint |

  

HttpServletRequest#login(String username, String password)  
HttpServletRequest#authenticate(HttpServletResponse response)  
HttpServletRequest#logout()
  
  
자세한 내용은 다음 첨부파일을 참고 : 

[##\_1C|cfile10.uf.1441E0364EF424A420D241.pdf|filename="servlet3.0-javaone.pdf" filemime="application/pdf"|\_##]

