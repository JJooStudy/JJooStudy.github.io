---
layout: post
title:  "DOM(Document Object Model)-2 HTMLElement"
author: "JJoo"
comments: true
tags: javescript
---


## HTMLElement

![DOM_HTMLElement](/images/img_DOM_HTMLElement.png)


getElement\* 메소드를 통해서 원하는 객체를 조회했다면 이 객체들을 대상으로 구체적인 작업을 처리해야 한다. 

이를 위해서는 획득한 객체가 무엇인지 알아야 한다. 그래야 적절한 메소드나 프로퍼티를 사용할 수 있다.

아래 코드는 getElement\*의 리턴 값을 보여준다.

```html
<ul>
	<li>HTML</li>
	<li>CSS</li>
	<li id="active">JavaScript</li>
</ul>
<script>
	var li = document.getElementById('active');
	console.log(li.constructor.name);
	var lis = document.getElementsByTagName('li');
	console.log(lis.constructor.name);
</script>
```


결과
```
HTMLLIElement //html li element
HTMLCollection // 복수/유사배열
```
이것을 통해서 알 수 있는 것은 아래와 같다.
• ```document.getElementById``` : 리턴 데이터 타입은 ```HTMLLIELement```
• ```document.getElementsByTagName``` : 리턴 데이터 타입은 ```HTMLCollection```
즉 실행결과가 하나인 경우 ```HTMLLIELement```, 복수인 경우 ```HTMLCollection```을 리턴하고 있다. 


실행결과가 하나인 엘리먼트들을 좀 더 살펴보자

```html
<a id="anchor" href="http://opentutorials.org">opentutorials</a>
<ul>
	<li>HTML</li>
	<li>CSS</li>
	<li id="list">JavaScript</li>
</ul>
<input type="button" id="button" value="button" />
<script>
	var target = document.getElementById('list');
	console.log(target.constructor.name);
	
	var target = document.getElementById('anchor');
	console.log(target.constructor.name);
	
	var target = document.getElementById('button');
	console.log(target.constructor.name);
</script>
```

결과
```
HTMLLIElement : html li element
HTMLAnchorElement : html anchor element
HTMLInputElement : html input element 
```

이를 통해서 알 수 있는 것은 엘리먼트의 종류에 따라서 리턴되는 객체가 조금씩 다르다는 것이다. 
엘리먼트마다 리턴되는 객체가 다르고 그 객체의 속성에 접근해 제어할 수 있다. 

참고 https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-6043025



#### HTMLLIElement

https://www.w3.org/TR/2003/REC-DOM-Level-2-HTML-20030109/html.html#ID-74680021

```
interface HTMLLIElement : HTMLElement {
	attribute DOMString type;
	attribute long value;
};
```


#### HTMLAnchroElement

https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-48250443

```
interface HTMLAnchorElement : HTMLElement {
	attribute DOMString accessKey;
	attribute DOMString charset;
	attribute DOMString coords;
	attribute DOMString href;
	attribute DOMString hreflang;
	attribute DOMString name;
	attribute DOMString rel;
	attribute DOMString rev;
	attribute DOMString shape;
	attribute long tabIndex;
	attribute DOMString target;
	attribute DOMString type;
	void blur();
	void focus();
};
```


#### HTMLInputElement : html input element 

https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-6043025

```
interface HTMLInputElement : HTMLElement {
           attribute DOMString       defaultValue;
           attribute boolean         defaultChecked;
  readonly attribute HTMLFormElement form;
           attribute DOMString       accept;
           attribute DOMString       accessKey;
           attribute DOMString       align;
           attribute DOMString       alt;
           attribute boolean         checked;
           attribute boolean         disabled;
           attribute long            maxLength;
           attribute DOMString       name;
           attribute boolean         readOnly;
  // Modified in DOM Level 2:
           attribute unsigned long   size;
           attribute DOMString       src;
           attribute long            tabIndex;
  // Modified in DOM Level 2:
           attribute DOMString       type;
           attribute DOMString       useMap;
           attribute DOMString       value;
  void               blur();
  void               focus();
  void               select();
  void               click();
};
```


즉 엘리먼트 객체에 따라서 프로퍼티가 다르다는 것을 알 수 있다. 하지만 모든 엘리먼트들은 HTMLElement를 상속 받고 있다. 
```
interface HTMLLIElement : HTMLElement {
interface HTMLAnchorElement : HTMLElement {
```
```HTMLElement```는 아래와 같다.
```
interface HTMLElement : Element {
		attribute DOMString id;
		attribute DOMString title;
		attribute DOMString lang;
		attribute DOMString dir;
		attribute DOMString className;
};
```
















