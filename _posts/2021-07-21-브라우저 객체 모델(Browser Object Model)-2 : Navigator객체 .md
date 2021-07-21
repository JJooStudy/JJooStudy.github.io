---
layout: post
title:  "BOM(Browser Object Model)-2"
author: "JJoo"
comments: true
tags: Javascript
---


## Location 객체 


Location 객체는 문서의 주소와 관련된 객체로 Window 객체의 프로퍼티다. 

이 객체를 이용해서 윈도우의 문서 **URL을 변경**할 수 있고, **문서의 위치와 관련해서 다양한 정보를 얻을** 수 있다.


#### 현재 윈도우의 URL 알아내기

```javascript
console.log(location.toString(), location.href);
```


`console.log(location)` : location 객체에 대한 정보가 나옴 

`alert(location)`: location url 주소만 알럿창에 띄워짐 


`console`로 찍으면 해당 객체의 정보에 접근해서 그 정보를 풀어주지만 `alert`는 `alert`으로 띄우는 정보가 문자여야 하기 때문에 `location` 정보의 **문자화**인 `url` 정보를 보여주고 이는 `location.toString()`값과 같다 



#### URL Parsing

```javascript
console.log(location.protocol, location.host, location.port, location.pathname, location.search, location.hash)
```


`location.protocol` : http: 

`location.host` : 도메인 ex) inflearn.com 

`location.port` : undefined(80), 8080, 8081 … 

`location.pathname` : host 뒤에 붙는 구체적인 정보 ex) /course/javascript

`location.search` : ?뒤에 따라오는 정보들 표출 

`location.hash` : #뒤에 오는 정보 출력 

![Location 객체]('/img_BOM_location.png' Location 객체)




#### URL 변경하기 

아래 코드는 현재 문서를 http://egoing.net으로 이동한다.

`location.href = 'http://egoing.net';`

`location.href` 는 **읽기**도 가능하고 정보를 **입력해 이동**도 가능하다.


아래와 같은 방법도 같은 효과를 낸다.

`location = 'http://egoing.net';`

아래는 현재 문서를 리로드하는 간편한 방법을 제공한다.

`location.reload();`


