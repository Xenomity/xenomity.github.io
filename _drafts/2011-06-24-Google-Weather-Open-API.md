---
title: "Google Weather Open API"
tags: [google-weather, open-api, web-service]
published: 2011-06-24T04:30:00+09:00
---

Google에서 제공하는 날씨 정보 API로 4일간의 일기예보 및 해당 날씨의 이쁜(?) 아이콘까지 제공한다.  
현재까지 Weather API는 라이센스 키의 인증없이 공개적인 사용이 가능하다. :)

http://www.google.co.kr/ig/api?weather=<u>{도시명</u><u>}<br>
</u>
  
예) HTTP v1.1 http://www.google.co.kr/ig/api?weather=seoul  

    \<?xml version="1.0"?\> \<xml\_api\_reply version="1"\>     \<weather section="0" row="0" mobile\_zipped="1" mobile\_row="0" tab\_id="0" module\_id="0"\>          \<!-- 날씨 정보 --\>     \<forecast\_information\>         \<city data="seoul"/\>         \<postal\_code data="seoul"/\>         \<latitude\_e6 data=""/\>         \<longitude\_e6 data=""/\>         \<forecast\_date data="2011-06-16"/\>         \<current\_date\_time data="2011-07-15 19:29:56 +0000"/\>         \<unit\_system data="SI"/\>     \</forecast\_information\>     \<!-- 현재 기후 --\>     \<current\_conditions\>         \<condition data="비"/\>         \<temp\_f data="70"/\>  //화씨         \<temp\_c data="21"/\>  //섭씨         \<humidity data="습도: 94%"/\>         \<icon data="/ig/images/weather/rain.gif"/\>  //날씨 이미지 아이콘         \<wind\_condition data="바람: 북풍, 8 km/h"/\>     \</current\_conditions\>     \<!-- 기후 정보 --\>     \<forecast\_conditions\>         \<day\_of\_week data="토"/\>  //요일         \<low data="23"/\>  //최저기온         \<high data="28"/\>  //최고기온         \<icon data="/ig/images/weather/thunderstorm.gif"/\>  //날씨 이미지 아이콘         \<condition data="강우(천둥, 번개 동반)"/\>     \</forecast\_conditions\>     ... {오늘 이후 날씨 데이터}     \</weather\> \</xml\_api\_reply\>

  