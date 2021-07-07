---
layout: post
title:  "표준 내장 객체(Standard Built-in Object)"
author: "JJoo"
comments: true
tags: Javascript
---




## 표준 내장 객체(Standard Built-in Object)

> 자바스크립트가 기본적으로 가지고 있는(탑재되어있는) 객체들 


#### 자바스크립트의 내장객체

• Object

• Function

• Array

• String

• Boolean

• Number

• Math

• Date

• RegExp


자바스크립트라는 언어 자체(코어)가 제공하는 내장 객체는 저 9가지 항목이 전부이지만 호스트환경이 제공하는 api는 훨씬 많다.  


> 자바스크립트 호스트 환경이란?
> 
> 자바스크립트가 구동되는 환경 
> 
> 브라우저, 웹서버(node.js … ), 

자바스크립트 내장 객체를 이용할 수도 있고 필요에 따라서 내장 객체 이외의 메소드를 추가하거나 원하는 객체를 만들 수 있는 데 이를 '사용자 정의 객체'라고 한다.



## 배열을 확장 

혼자서 존재하는 함수의 쓰임을 잘 이해할 수 있어야 하기 때문에 함수명을 지을 때 잘 지어야한다. 



과제 : `('seoul','new york','ladarkh','pusan', 'Tsukuba')` 중 하나를 랜덤하게 뽑는 기능이 필요함.


```javacsript
var arr = new Array('seoul','new york','ladarkh','pusan', 'Tsukuba');
function getRandomValueFromArray(haystack){
var index = Math.floor(haystack.length*Math.random());
return haystack[index]; 
}
console.log(getRandomValueFromArray(arr));
```


결과 

`Array` 배열에 들어있는 값들이 랜덤으로 찍힌다.

`getRandomValueFromArray`를 실행하면 `Array` 안에 들어있는 값을 랜덤으로 보여준다.


- `Math.random()` : 값을 랜덤하게 뽑아낼때 쓰는 메소드 0<1 사이의 소수(0.1, 0.12 .. 0.99… )를 제공
- ex) `arr.length*Math.random()` = 최소 0부터 최대 4까지의 임의의 소수를  랜덤하게 제공
- `.floor()` 소수점을 제거하는 메소드. 소수를 소수점을 제거해서 정수로 제공 



#### 배열의 확장 2 

과제 : 위 코드에서 `Array`라는 생성자를 확장해서 모든 배열이 그 배열이 가지고 있는 특정한 값을 랜덤하게 뽑을 수 있는 코드로 수정 


```javacsript
/*
기존코드 

var arr = new Array('seoul','new york','ladarkh','pusan', 'Tsukuba');

function getRandomValueFromArray(haystack){
  var index = Math.floor(haystack.length*Math.random());
  return haystack[index]; 
}

console.log(getRandomValueFromArray(arr));
*/

Array.prototype.rand = function(){
  var index = Math.floor(this.length*Math.random());
  return this[index];
}

var arr = new Array('seoul','new york','ladarkh','pusan', 'Tsukuba');

console.log(arr.rand());

```


- `Array` : 배열을 만들기 위한 생성자 함수
- `Array`라는 생성자 함수에 `prototype`에 `rand()`라는 메소드를 추가한다. 이 `rand()`메소드 안에서의 `this`는 `Array`를 생성자로 만든 배열 객체 자체를 가리킨다. (ex. `arr` )
- `Array`라는 배열에 `prototype`이라는 프로퍼티에 소속되어 있는 rand()라는 것이 한눈에 보여서 기존의 getRandomValueFromArray()보다 가독성이 좋아진다. 
- 또 `rand()`는 인자를 받고 있는 형태가 아니기 때문에 사용자가 신경을 덜 써도 되므로 좀더 편하다. 
- `prototype`을 확장해서 모든 배열이 공통적으로 가지고 있어야 하는 `api`를 직접 정의할 수 있다. 공통적으로 정의해야 하는 기능에 활용하면 유용하다. 




