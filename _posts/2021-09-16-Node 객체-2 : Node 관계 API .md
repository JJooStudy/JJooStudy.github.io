---
layout: post
title:  "Node 객체-2 : Node 관계 API"
author: "JJoo"
comments: true
tags: javescript
---


## Node 관계 API 

Node 객체는 Node 간의 관계 정보를 담고 있는 일련의 API를 가지고 있다. 다음은 관계와 관련된 프로퍼티들이다.

- Node.childNodes
- 
자식노드들을 유사배열에 담아서 리턴한다.

- Node.firstChild

: 첫번째 자식노드

- Node.lastChild

: 마지막 자식노드

- Node.nextSibling

: 다음 형제 노드

- Node.previousSibling

: 이전 형제 노드


```html
<body id="start">
<ul>
	<li><a href="./532">html</a></li> 
	<li><a href="./533">css</a></li>
	<li><a href="./534">JavaScript</a>
		<ul>
			<li><a href="./535">JavaScript Core</a></li>
			<li><a href="./536">DOM</a></li>
			<li><a href="./537">BOM</a></li>
		</ul>
	</li>
</ul>
<script>
	var s = document.getElementById('start');
	console.log(1, s.firstChild); // #text
	var ul = s.firstChild.nextSibling
	console.log(2, ul); // ul
	console.log(3, ul.nextSibling); // #text
	console.log(4, ul.nextSibling.nextSibling); // script
	console.log(5, ul.childNodes); //text, li, text, li, text, li, text
	console.log(6, ul.childNodes[1]); // li(html)
	console.log(7, ul.parentNode); // body
</script>
</body>
```

```console.log(1, s.firstChild); // #text ```

: ```<body id="start">``` 와 ```<ul>``` 사이의 영역. 공백, 엔터 모두 포함한다. 만약 ```<body id="start">``` 와 ```<ul>``` 사이에 공백 또는 엔터가 없이 붙어있다면 
```console.log(1, s.firstChild)``` 할 시 ```<ul></ul>```이 된다. 
