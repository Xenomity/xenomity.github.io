---
title: "Oracle USER_SOURCE Table"
tags: [database, oracle]
date: 2011-01-03T21:58:00+09:00
---

오라클 DB의 USER\_SOURCE 테이블을 통해 간단히 Function 및 Procedure, Package의 코드를 확인할 수 있다.

| Column | Data   |
|--------|--------|
| NAME   | 소스명 |
| TYPE   | 소스 타입 (FUNCTION, PACKAGE, PACKAGE BODY, PROCEDURE, TYPE) |
| LINE   | 소스 줄 수 |
| TEXT   | 소스 내용 |

  
위와 같이 4가지 컬럼이 있으며, 몇가지 예를 보자면..  
  
```sql
SELECT TEXT FROM USER_SOURCE
WHERE NAME = 'GET_SERIES_ABATTEND' AND TYPE = 'FUNCTION' ORDER BY LINE;
```

```
TEXT  
-------------------------------------------------------------------------  
FUNCTION      GET_SERIES_ABATTEND (    
         IN_YYMM IN VARCHAR2,    
         IN_CONTRACT_NO IN VARCHAR2,    
         IN_PROJECT_NO IN VARCHAR2,    
         IN_PCHASU IN VARCHAR2) RETURN NUMBER   
  AS      
      attend_days NUMBER(2) := 0;      
  
...  
```
  
```sql
SELECT * FROM USER_SOURCE
WHERE NAME = 'GET_SERIES_ABATTEND' AND TYPE = 'FUNCTION' ORDER BY LINE;
```

```
 NAME                TYPE     LINE TEXT  
 ------------------- -------- ---- --------------------------------------  
 GET_SERIES_ABATTEND FUNCTION    1 FUNCTION      GET_SERIES_ABATTEND (    
 GET_SERIES_ABATTEND FUNCTION    2         IN_YYMM IN VARCHAR2,    
 GET_SERIES_ABATTEND FUNCTION    3         IN_CONTRACT_NO IN VARCHAR2,    
 GET_SERIES_ABATTEND FUNCTION    4         IN_PROJECT_NO IN VARCHAR2,    
 GET_SERIES_ABATTEND FUNCTION    5         IN_PCHASU IN VARCHAR2) RETURN  
 
...
```

```sql
SELECT DISTINCT(NAME) FROM USER_SOURCE
WHERE TYPE = 'PROCEDURE'
```

```  
 NAME  
 ---------------------  
 DEL_ADJUST_CB_PROCESS  
 DEL_ADJUST_PB_PROCESS  
 INS_ADJUST_CB_PROCESS  
  
...  
```
  
