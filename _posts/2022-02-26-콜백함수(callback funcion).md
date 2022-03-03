---
layout: post
title:  "콜백함수(callback funcion)"
author: "JJoo"
comments: true
tags: Javascript
---

##  콜백함수란? 

callback : 회신하다/답신하다 

callback function : 회신되는 함수

콜백함수란 내가 넘기고자 하는 대상에게 제어권을 위임하는 것이다. 그럼 제어권을 넘겨받은 대상이 전적으로 관여하게 된다. 

## 제어권 위임 

3가지에 대한 제어권을 위임한다. 

- 실행시점
- 매개변수
- this

#### 실행시점

주기함수를 호출 하면 milliseconds단위의 주기마다 콜백 함수를 알아서 실행한다. 

`주기함수호출(인자 1: 콜백함수, 인자2: 주기(milliseconds));`

`setInterval(callback, milliseconds);`

```javascript 
setInterval(function(){
	console.log('1초마다 실행')
}, 1000);
```

위 코드는 제어권을 `setInterval`에게 넘겨주는 것이 된다. 

#### 매개변수 

`forEach`는 배열에 있는 요소들을 순서대로 하나씩 꺼내서 **첫번째 매개변수에 값, 두번째 매개변수에 index를 부여**하면서 함수를 실행한다. 

`forEach호출( 인자1: 콜백함수[, 인자2: this로 인식할 대상(생략가능)])`

`forEach(callback, thisArg)`

여기서 `callback`은 매개변수로 아래 3가지를 받는 다. 

- currentValue
    - 배열에서 현재 처리 중인 요소
- index
    - 배열에서 현재 처리 중인 요소의 인덱스
- array
    - `forEach()`가 적용되고 있는 배열

thisArg는 선택사항으로 callback을 실행할 때 this로서 사용하는 값이 들어간다. 

반환값으로는 undefined로 받는다. 

```javascript 
var arr = [1, 2, 3, 4, 5];
var entries = [];
arr.forEach(function(value, index){
	entries.push([index, value, this[index]]);
}, [10, 20, 30, 40, 50]);
console.log(entries); // [[0, 1, 10], [1, 2, 20], [2, 3, 30], [3, 4, 40], [4, 5, 50]]
```

이렇게 forEach는 여러 사항이 정의가 되어 있기 때문에 forEach를 사용할 때는 이 정의된 규칙에 맞춰서 사용해야 한다.

내가 다르게 쓰고 싶다고 해서 다르게 바꿀 수 있는 것이 아니다. 

#### this

`addEventListener`는 콜백함수를 받을 때 this는 eventTarget으로 하고 콜백함수의 첫번째 인자에는 event 객체를 넘겨주도록 정의 되어 있다. 

`target.addEventListener(type, listener[, options]);`

`target.addEventListener(type, listener[, useCapture]);`

`target.addEventListener(type, callback, options)`

- `type`
    - 이벤트 유형 (click, change 등)
- `listener`
    - 콜백함수
        - 단일 매개변수 허용
        - 발생한 이벤트를 설명하는 event에 기반한 객체
        - 반환값이 없다.
        - this
            - argument의 currentTarget 속성값이 정의
- `options`
    - 다양한 옵션을 담은 객체
    - useCapture라는 캡쳐 여부에 관련한 boolean값

```javascript 
document.body.innerHTML = '<div id="abc">abc</div>';
function cbFunc(x){
	console.log(this, x); // <div id="abc">abc</div>, PointerEvent { ... }
}
document.getElementById('abc').addEventListener('click', cbFunc);
// 여기서 this을 다르게 주고 싶다면 아래처럼 bind 사용 
var obj = {a: 1};
document.getElementById('abc').addEventListener('click', cbFunc.bind(obj));
```

## 콜백함수의 특징 

- 함수(A)의 인자로 콜백함수(B)를 전달하면 A가 B에 대한 제어권을 갖게 된다.
- 특별한 요청이 없는 한, A에 미리 정해 놓은 방식에 따라 B를 호출한다.
- 미리 정해 놓은 방식이란, 어떤 시점에서 콜백을 호출할 지, 인자에는 어떤 값들을 지정할지, this에 무엇을 바인딩 할 지 등이다.

## 주의! 콜백은 함수다. 

콜백은 함수이므로 this는 기본적으로 전역객체를 가리킨다.

```javascript 
var arr = [1, 2, 3, 4, 5];
var obj = {
	vals: [1, 2, 3],
	logValues: function(v, i){
		if(this.vals){
			console.log(this.vals, v, i);
		} else {
			console.log(this, v, i);
		}
	}
}
obj.logValues(1, 2); // 매개변수로 호출했기때문에 this는 obj
arr.forEach(obj.logValues); // 콜백함수로 전달했기 때문에 this는 window 
// this로 명시하고 싶은 것을 bind 시켜주거나 두번째 인자 thisArg로 this를 넘겨준다
arr.forEach(obj.logValues.bind(obj));
arr.forEach(obj.logValues, obj);
```

## Reference 
- 인프런 코어 자바스크립트 