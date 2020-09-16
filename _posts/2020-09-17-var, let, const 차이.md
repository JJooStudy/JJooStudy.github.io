---
layout: post
title:  "var, let, const 차이"
author: "JJoo"
comments: true
---

Var  : function-scoped
Let, const : block-scoped

* 함수 스코프(Function scope)

 : 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.

* 블록 스코프(Block scope)
 : 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.


## 1. var(function-scoped)

: var 는 function 단위로 변수 hoisting 현상 발생
: var 를 사용할 경우 의도하지 않은 변수값의 변경이 일어날 가능성이 큼
: var는 변수 재 선언이 가능 

* 변수 호이스팅이란?

: 변수 선언시 해당 변수가 속한 범위(scoped) 최상단으로 올려버리는 현상

{% highlight javascript %}
var n = 1;
function test() {
  var n; // hoisting
  console.log(n); //undefined
  n = 2;
  console.log(n); // 2
}
test();
Console.log(n); // 1


var a = 'asf'
var a = 'qwr'
// 이미 있는 변수 이름으로 재선언해도 에러 발생 X

c = 'zxvc'
var c
console.log( c ) // var c 가 hoisting되서 zxcv 로 출력 
{% endhighlight %}


## 2. 	2. let, const ( block-scoped )

: let, const 는 변수 재선언이 불가능
: let은 변수 재할당이 가능, const는 변수 재할당도 불가능 

{% highlight javascript %}
let a = 'asd'
let a = 'qwr' // a 가 이미 선언되어 있어서 불가능
a = 'zxc' // 가능 

const a = 'asd'
const a = 'qwe' // a가 이미 선언되어 있어서 불가능
a = 'zxc' // 변수가 이미 할당되어 있어서 불가능 
{% endhighlight %}

: let, const 는 block 단위로 hoisting 발생 

{% highlight javascript %}
function findUser(id) {
  if (id > 0) {
    let successMsg = "사용자를 조회하였습니다."
    console.log(successMsg)
    console.log({ id: id, name: "사용자 #" + id })
  } else {
    let failureMsg = "잘못된 아이디입니다!"
    console.log(failureMsg)
  }
  console.log("실패 메세지:", failureMsg) // ReferenceError: failureMsg is not defined
}
{% endhighlight %}

* let 대신 var였다면 undefined -> failureMsg 가 선언된 적이 없어도 상단으로 hoisting 되서 에러가 안뜨고undefined 출력함 

{% highlight javascript %}
c = 'asd'  //declaredReferenceError: c is not defined
let c 
{% endhighlight %}

* let값을 할당하기 전에 변수가 선되어서 에러가 발생 

{% highlight javascript %}
let a 
a = 'asf' // let 은 선언하고 나중에 값을 할당 할 수 있음 

const aa = 'asd' // const 는 선언과 동시에 값을 할당해야 함 
{% endhighlight %}
