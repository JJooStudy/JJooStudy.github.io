---
layout: post
title:  "표준 내장 객체(Standard Built-in Object)"
author: "JJoo"
comments: true
tags: Javascript
---


## object의 object

object 객체는 객체의 가장 기본적인 형태를 가지고 있는 객체다. (모든 객체의 최초의 조상)

즉 아무것도 상속받지 않는 순수한 객체.

자바스크립트에서는 값을 저장하는 기본적인 단위로 object를 사용한다.


```javascript
var grades = { 'laura': 10, 'jessie': 11 }
```

자바스크립트의 모든 객체는 `object` 객체를 상속받는다.

`object` 객체는 모든 객체의 부모가 되서 `object`의 프로토타입은 모든 객체의 프로토타입, 프로퍼티가 된다. 

`object` 객체의 `prototype`은 모든 객체의 프로토타입이 되기때문에 

`object` 객체를 확장하면 모든 객체에 접근할 수 있는 api를 만들 수 있다. 



## object API 사용법 

`object` 객체에는 두가지 형태의 메소드가 있다. 

프로토타입이 있는 것과 없는 것

ex)

`object.keys()` : object가 가지고 있는 키값을 배열로 나열해주는 메소드

`object.prototype.toString()` : 객체가 담고 있는 어떠한 값이나 상태가 무엇인가를 사람이 보기 좋게 보여주는 메소드

```javascript
//object.keys() 
var arr = ["a", "b", "c"];
console.log('Object.keys(arr)', Object.keys(arr));
// 결과 [0, 1, 2]

//object.prototype.toString()
var o = new Object();
console.log( o.string());
// 결과 [object, object]

var a = new Array(1,2,3);
console.log(a.toString());
// 결과 1,2,3
```


프로토타입이 있는 것은 `object.prototype`으로 정의 되어 있는 것, 프로토타입이 없는 것은 `object`의 프로퍼티로 정의되어 있는 것이다.

그래서 `object.prototype`은 모든 객체가 다 상속하므로 어떠한 객체를 생성자 함수로 생성하고(`new object()`) 그 생성한 객체에 메소드로 사용할 수 있다. (객체.메소드() : `o.toString()`)

프로토타입이 없는 것은  별도의 함수처럼 `object.메소드(인자)`의 형태로 인자를 받아서 처리한다.

`object.prototype.~` -> 모든객체가 활용가능

`object.~` - > 하위객체는 사용불가능



## object의 확장 


ex)
```javascript
Object.prototype.contain = function(neddle) {
    for(var name in this){
        if(this[name] === neddle){
            return true;
        }
    }
    return false;
}

var o = {'name':'laura', 'city':'seoul'}

console.log(o.contain('laura')); 

var a = ['laura','jessie','lia'];
console.log(a.contain('jessie'));
```


`.contain()` : `o`에 `laura`가 포함되어 있는 지 검사해서 `true`/`false` 값을 결과로 보여줌 



