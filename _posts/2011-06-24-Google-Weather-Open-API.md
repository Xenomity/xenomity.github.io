---
title: "Google Weather Open API"
tags: [google-weather, open-api, web-service]
published: 2011-06-24T04:30:00+09:00
---

Google에서 제공하는 날씨 정보 API로 4일간의 일기예보 및 해당 날씨의 이쁜(?) 아이콘까지 제공한다.  
현재까지 Weather API는 라이센스 키의 인증없이 공개적인 사용이 가능하다. :)

```
http://www.google.co.kr/ig/api?weather={도시명}
```
  
예) HTTP v1.1 http://www.google.co.kr/ig/api?weather=seoul  
```xml
<?xml version="1.0"?>
<xml_api_reply version="1">
    <weather section="0" row="0" mobile_zipped="1" mobile_row="0" tab_id="0" module_id="0">
     
    <!-- 날씨 정보 -->
    <forecast_information>
        <city data="seoul"/>
        <postal_code data="seoul"/>
        <latitude_e6 data=""/>
        <longitude_e6 data=""/>
        <forecast_date data="2011-06-16"/>
        <current_date_time data="2011-07-15 19:29:56 +0000"/>
        <unit_system data="SI"/>
    </forecast_information>
 
    <!-- 현재 기후 -->
    <current_conditions>
        <condition data="비"/>
        <temp_f data="70"/>  //화씨
        <temp_c data="21"/>  //섭씨
        <humidity data="습도: 94%"/>
        <icon data="/ig/images/weather/rain.gif"/>  //날씨 이미지 아이콘
        <wind_condition data="바람: 북풍, 8 km/h"/>
    </current_conditions>
 
    <!-- 기후 정보 -->
    <forecast_conditions>
        <day_of_week data="토"/>  //요일
        <low data="23"/>  //최저기온
        <high data="28"/>  //최고기온
        <icon data="/ig/images/weather/thunderstorm.gif"/>  //날씨 이미지 아이콘
        <condition data="강우(천둥, 번개 동반)"/>
    </forecast_conditions>
 
    ... {오늘 이후 날씨 데이터}
 
    </weather>
</xml_api_reply>
```
  
