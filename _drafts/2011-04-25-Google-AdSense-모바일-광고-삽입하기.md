---
title: "Google AdSense 모바일 광고 삽입하기"
tags: adsense
date: 2011-04-25T10:58:28+09:00
---

Google AdSense에서 제공하는 모바일 컨텐츠용 광고 단위를 이용하면 AdMob같이 별도 라이브러리를 필요로하는 프로바이더보다 손쉽게 광고를 추가할 수 있다.  
  
구글 애드센스 : [http://www.google.com/adsense](http://www.google.com/adsense)  
내 광고 -\> 모바일 컨텐츠 -\> 광고 단위 -\> 새 광고 단위  
  
예) 광고 단위 추가로 생성된 코드

    \<script type="text/javascript"\>\<!--   // XHTML should not attempt to parse these strings, declare them CDATA.   /\* \<![CDATA[\*/   window.googleAfmcRequest = {     client: '########################',     format: '320x50\_mb',     output: 'html',     slotname: '##############',   };   /\*]]\> \*/ //--\>\</script\> \<script type="text/javascript"    src="http://pagead2.googlesyndication.com/pagead/show\_afmc\_ads.js"\> \</script\>

  
Layout XML에서 광고 영역 스타일을 잡아주고 WebView를 통해 상기 코드를 적용하면 끝~ -0-  
(참고로 당근 android.permission.INTERNET 권한이 AndroidManifest.xml에 추가되어 있어야 한다.)  
  
예) TestActivity.java  

    public class TestActivity extends Activity { public void onCreate(Bundle saveInstanceState) { ... WebView adsenseView = (WebView) findViewById(R.id.ads); adsenseView.getSettings().setJavaScriptEnabled(true); adsenseView.loadData( ~~~AdSense Code~~~ , "text/html", "UTF-8"); } }

  

[##\_1C|cfile29.uf.272C9C435301982924EC60.jpg|width="285" height="207" filename="201104251055.jpg" filemime="image/jpeg"|\_##]

-AdSense 테스트 결과-  

