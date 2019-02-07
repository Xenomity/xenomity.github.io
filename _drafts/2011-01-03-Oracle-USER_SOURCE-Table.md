---
title: "Oracle USER_SOURCE Table"
tags: [database, oracle]
date: 2011-01-03T21:58:00+09:00
---

오라클 DB의 USER\_SOURCE 테이블을 통해 간단히 Function 및 Procedure, Package의 코드를 확인할 수 있다. 본인처럼 귀차니즘(;;)으로 전용 DB툴을 설치하지 않고 JDBC로 직접 붙어 개발하는 경우에 편리하다. -0-ㅋ

| ** 컬럼명** | ** 데이터** |
|  NAME |  소스명 |
|  TYPE |  소스 타입 (FUNCTION, PACKAGE, PACKAGE BODY, PROCEDURE, TYPE) |
|  LINE |  소스 줄 수 |
|  TEXT |  소스 내용 |

  
위와 같이 4가지 컬럼이 있으며, 몇가지 예를 보자면..  
  

<u><span style="FONT-FAMILY: Courier New">SELECT TEXT FROM USER_SOURCE</span><br>
<span style="FONT-FAMILY: Courier New">WHERE NAME = 'GET_SERIES_ABATTEND' AND TYPE = 'FUNCTION' ORDER BY LINE;</span></u>  
  
TEXT  
-------------------------------------------------------------------------  
FUNCTION      GET\_SERIES\_ABATTEND (    
         IN\_YYMM IN VARCHAR2,    
         IN\_CONTRACT\_NO IN VARCHAR2,    
         IN\_PROJECT\_NO IN VARCHAR2,    
         IN\_PCHASU IN VARCHAR2) RETURN NUMBER   
  AS      
      attend\_days NUMBER(2) := 0;      
  
...  

  

<u><span style="FONT-FAMILY: Courier New">SELECT * FROM USER_SOURCE</span><br>
<span style="FONT-FAMILY: Courier New">WHERE NAME = 'GET_SERIES_ABATTEND' AND TYPE = 'FUNCTION' ORDER BY LINE;</span><br>
</u>  
 NAME                TYPE     LINE TEXT  
 ------------------- -------- ---- --------------------------------------  
 GET\_SERIES\_ABATTEND FUNCTION    1 FUNCTION      GET\_SERIES\_ABATTEND (    
 GET\_SERIES\_ABATTEND FUNCTION    2         IN\_YYMM IN VARCHAR2,    
 GET\_SERIES\_ABATTEND FUNCTION    3         IN\_CONTRACT\_NO IN VARCHAR2,    
 GET\_SERIES\_ABATTEND FUNCTION    4         IN\_PROJECT\_NO IN VARCHAR2,    
 GET\_SERIES\_ABATTEND FUNCTION    5         IN\_PCHASU IN VARCHAR2) RETURN  
  
...
  

<u><span style="FONT-FAMILY: Courier New"><span style="FONT-FAMILY: Courier New">SELECT DISTINCT(NAME) FROM USER_SOURCE </span></span><span style="FONT-FAMILY: Courier New"><span style="FONT-FAMILY: Courier New">WHERE TYPE = 'PROCEDURE'</span></span></u>  
  
 NAME  
 ---------------------  
 DEL\_ADJUST\_CB\_PROCESS  
 DEL\_ADJUST\_PB\_PROCESS  
 INS\_ADJUST\_CB\_PROCESS  
  
...  

  
