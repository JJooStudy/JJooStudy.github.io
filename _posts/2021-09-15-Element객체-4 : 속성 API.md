---
layout: post
title:  "Element객체-4 : 속성 API"
author: "JJoo"
comments: true
tags: javescript
---


## 속성 API

속성(attribute)은 태그명으로는 부족한 부가적인 정보라고 할 수 있다. 


## 속성 제어 API 

각각의 기능은 이름을 통해서 유추할 수 있다. 

- Element.getAttribute(name)

- Element.setAttribute(name, value)

- Element.hasAttribute(name);

- Element.removeAttribute(name);


```html
<a id="target" href="http://opentutorials.org">opentutorials</a>
<script>
	var t = document.getElementById('target');
	console.log(t.getAttribute('href')); //http://opentutorials.org
	t.setAttribute('title', 'opentutorials.org'); // title 속성의 값을 설정한다.
	console.log(t.hasAttribute('title')); // true, title 속성의 존재여부를 확인한다.
	t.removeAttribute('title'); // title 속성을 제거한다.
	console.log(t.hasAttribute('title')); // false, title 속성의 존재여부를 확인한다.
</script>
```


모든 엘리먼트의 (HTML)속성은 (JavaScript 객체의) 속성과 프로퍼티로 제어가 가능하다.

속성 방식과 프로퍼티 방식은 차이가 있다. 

```html
<p id="target">
	Hello world
</p>
<script>
	var target = document.getElementById('target');
	// attribute 방식
	target.setAttribute('class', 'important');
	// property 방식
	target.className = 'important';
</script>
```

setAttribute('class', 'important')와 className = 'important'는 같은 결과를 만든다. 

하지만 전자는 attribute 방식(속성 방식)이고 후자는 property 방식이다. 

property 방식은 좀 더 간편하고 속도도 빠르지만 실제 html 속성의 이름과 다른 이름을 갖는 경우가 있어서 주의해야한다. 

이는 자바스크립트의 이름 규칙 때문이다.

|속성방식|property방식|
|---------|-------|
|class|className|
|readonly|readOnly|
|rowspan|rowSpan|
|colspan|colSpan|
|usemap|useMap|
|frameborder|framBorder|
|for|htmlFor|


심지어 속성과 프로퍼티는 값이 다를수도 있다. 아래 코드를 실행한 결과는 속성과 프로퍼티의 값이 꼭 같은 것은 아니라는 것을 보여준다.


```html
<a id="target" href="./demo1.html">ot</a>
<script>
	//현재 웹페이지가 http://localhost/webjs/Element/attribute_api/demo3.html 일 때 
	var target = document.getElementById('target');
	// http://localhost/webjs/Element/attribute_api/demo1.html 
	console.log('target.href', target.href);
	// ./demo1.html 
	console.log('target.getAttribute("href")', target.getAttribute("href"));
</script>
```


```target.href``` 형태의 프로퍼티방식으로 접근하면 절대경로로 표시가 되고 
```target.getAttribute("href") ```으로 접근하면 ```href```에 적혀있는 상대경로 그대로 표시된다. 


