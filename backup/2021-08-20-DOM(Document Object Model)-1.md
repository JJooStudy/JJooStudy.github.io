---
layout: post
title:  "DOM(Document Object Model)-1"
author: "JJoo"
comments: true
tags: Javascript
---


> 인프런 웹브라우저 Javascript 강의를 들으면서 정리한 내용입니다. 



DOM이란 Document Object Model로 웹페이지를 자바스크립트로 제어하기 위한 객체모델을 의미한다.

window객체의 document 프로퍼티를 통해서 사용할 수 있다. window객체가 창을 의미한다면 document객체는 윈도우에 로드된 문서를 의미한다고 할 수 있다. 


## 제어대상 찾기

문서를 자바스크립트로 제어하려면 제일 먼저 제어의 대상에 해당하는 객체를 찾아 그 대상에 대해서 작업을 한다.

document객체의 메소드를 이용하여 문서내에서 객체를 찾는다.


#### document.getElementsByTagName

getElementsByTagName은 인자로 전달된 **태그명**에 해당하는 **객체들**(복수)을 찾아서 그 리스트를 NodeList라는 유사 배열에 담아서 반환한다. 
NodeList는 배열은 아니지만 length와 배열접근연산자를 사용해서 엘리먼트를 조회할 수 있다.

```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
<script>
	var lis = document.getElementsByTagName('li');
	for(var i=0; i < lis.length; i++){
		lis[i].style.color='red'; 
	}
</script>
</body>
</html>
```

만약 조회의 대상을 좁히려면 아래와 같이 특정 객체를 지정하면 된다. 이러한 원칙은 다른 메소드에도 적용된다.


```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
	<ol>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ol>
<script>
	var ul = document.getElementsByTagName('ul')[0];
	var lis = ul.getElementsByTagName('li');
	for(var i=0; lis.length; i++){
		lis[i].style.color='red'; 
	}
</script>
</body>
</html>
```


#### document.getElementsByClassName

**class속성**의 값을 기준으로 객체를 조회할수도 있다.
이것도 유사배열에 담아서 반환한다.


```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li class="active">CSS</li>
		<li class="active">JavaScript</li>
	</ul>
<script>
	var lis = document.getElementsByClassName('active');
	for(var i=0; i < lis.length; i++){
		lis[i].style.color='red'; 
	}
</script>
</body>
</html>
```


#### document.getElementById

**id 값**을 기준으로 객체를 조회한다. 이도 유사배열로 반환하고 **복수가 아닌 하나의 결과만** 반환한다.
성능면에서 가장 우수하다.


```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li id="active">CSS</li>
		<li>JavaScript</li>
	</ul>
<script>
	var li = document.getElementById('active');
	li.style.color='red';
</script>
</body>
</html>
```


#### document.querySelector 

**css 선택자**의 문법을 이용해서 객체를 조회할수도 있다. 편리한 메소드이다.
querySelector는 해당되고 제일 먼저 찾게 되는 **하나만** 반환한다.

```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
	<ol>
		<li>HTML</li>
		<li class="active">CSS</li>
		<li>JavaScript</li>
	</ol>
<script>
	var li = document.querySelector('li');
	li.style.color='red';
	
	var li = document.querySelector('.active');
	li.style.color='blue';
</script>
</body>
</html>
```


#### 	document.querySelectorAll

querySelector과 기본적인 동작방법은 같지만 **모든 객체**를 조회한다는 점이 다르다.
**복수로** 반환한다.

```html
<!DOCTYPE html>
<html>
<body>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
	<ol>
		<li>HTML</li>
		<li class="active">CSS</li>
		<li>JavaScript</li>
	</ol>
<script>
	var lis = document.querySelectorAll('li');
	for(var name in lis){
		lis[name].style.color = 'blue';
	}
</script>
</body>
</html>
```
