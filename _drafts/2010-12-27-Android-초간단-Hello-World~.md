---
title: "Android 초간단 Hello World~"
tags: [android, eclipse]
date: 2010-12-27T15:58:28+09:00
---

\* 본 내용은 JDK 6, Eclipse 3.6, Android SDK Tools (rev.7) 기준으로 작성함.  
  
Android 개발환경 구축 선작업이 되어있지 않은 경우, [<font color="#6699cc">Android 개발환경 구축</font>](http://www.xenomity.com/entry/Android-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95) 포스트 참고.  
  
  

#### **1. Android Project 생성.**
New =\> Project =\> Android =\> Android Project  

[##\_1C|cfile22.uf.272CF541530192732E76B6.jpg|width="525" height="727" filename="201011231243.jpg" filemime="image/jpeg"|\_##]

  
   1) Project Name : 프로젝트명  

   2) Application Name : 어플리케이션명

   3) Package Names : 식별되는 Package명

   4) Create Activity : Application 단위의 기본 생명주기

   5) Min SDK Version : 최소 실행 가능한 SDK revision 번호 (ex. android 2.2 == revision 8)  
  

#### **2. Sample Code 작성.**

[##\_1C|cfile10.uf.254184415301928221D68F.jpg|width="247" height="232" filename="201011231247.jpg" filemime="image/jpeg"|\_##]

  

정상적으로 프로젝트가 생성되면 위와 같은 구조가 만들어진다.

test.helloworld.HelloWorld.java를 수정해보면,  

    package test.helloworld; import android.app.Activity; import android.os.Bundle; import android.widget.TextView; /\*\* \* Hello World \* \* @author Xenomity™ a.k.a P-slinc' (이승한) \*/ public class HelloWorld extends Activity { @Override public void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); TextView textView = new TextView(this); textView.setText("요요~~! 안드로이드!!"); setContentView(textView); } }

  
  

#### **3. 실행**
Run As =\> Android Application  

[##\_1C|cfile7.uf.25515D3E530192910C34B6.jpg|width="487" height="597" filename="201011231250.jpg" filemime="image/jpeg"|\_##]

정상적으로 실행된 모습  
  

