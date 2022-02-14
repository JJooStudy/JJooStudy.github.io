---
layout: post
title:  "BOM(Browser Object Model)-3: Navigator객체"
author: "JJoo"
comments: true
tags: Javascript
---


> 인프런 웹브라우저 Javascript 강의를 들으면서 정리한 내용입니다. 


## Navigator객체 

브라우저의 정보를 제공하는 객체다. 주로 호환성 문제등을 위해서 사용한다.

`console.dir(navigator)`


#### 주요한 프로퍼티


- appName

웹브라우저의 이름. IE는 Microsoft Internet Explorer, 파이어폭스, 크롬등은 Nescape로 표시한다.


- appVersion

브라우저의 버전. 

`"5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36"`

- userAgent

브라우저가 서버측으로 전송하는 USER-AGENT HTTP 헤더의 내용. appVersion과 비슷하다.

`"5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36"`

- platform

브라우저가 동작하고 있는 운영체제에 대한 정보.

"Win32"




#### 기능테스트 

내가 작성한 코드가 어떤 브라우저에서 사용하고자 하는 api가 있는 지, 없는 지 파악하는 것 

브라우저의 호환성을 위해 주로 사용한다. 

ex) `Object.keys`라는 메소드는 객체의 `key` 값을 배열로 리턴하는 `Object`의 메소드다. 

이 메소드는 ECMAScript5에 추가되었기 때문에 오래된 자바스크립트와는 호환되지 않는다. 아래의 코드를 통해서 호환성을 맞출 수 있다. 

```javascript
// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys

if (!Object.keys) { // Object.keys의 기능이 없다면 Object.keys을 추가해서 그 기능을 사용하게 함 

	Object.keys = (function () {
		'use strict';
		var hasOwnProperty = Object.prototype.hasOwnProperty,
			hasDontEnumBug = !({toString: null}).propertyIsEnumerable('toString'),
			dontEnums = [
				'toString',
				'toLocaleString',
				'valueOf',
				'hasOwnProperty',
				'isPrototypeOf',
				'propertyIsEnumerable',
				'constructor'
			],
			dontEnumsLength = dontEnums.length;
			
		return function (obj) {
			if (typeof obj !== 'object' && (typeof obj !== 'function' || obj === null)) {
				throw new TypeError('Object.keys called on non-object');
			}
			var result = [], prop, i;
			
			for (prop in obj) {
				if (hasOwnProperty.call(obj, prop)) {
					result.push(prop);
				}
			}
			if (hasDontEnumBug) {
				for (i = 0; i < dontEnumsLength; i++) {
					if (hasOwnProperty.call(obj, dontEnums[i])) {
						result.push(dontEnums[i]);
					}
				}
			}
			return result;
		};
	}());
}
```
