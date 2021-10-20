---
layout: post
title:  "Arguments"
author: "JJoo"
comments: true
tags: Javascript
---


## Arguments

함수에는 arguments라는 변수에 담긴 숨겨진 유사 배열이 있다. 이 배열에는 함수를 호출할 때 입력한 인자가 담겨있다. 

> arguments는 함수안에서 사용할 수 있도록 그 이름이나 특성이 약속되어 있는 일종의 배열이다. 

(자바스크립트와 약속되어있는 특수한 이름의 변수명. 
그 안에는 arguments라는 유사객체, 유사배열이 담겨있다
사용자가 전달한 인자가 그 안에 담긴다.
Arguments를 통해서 사용자가 전달한 인자들에 접근할 수 있는 기능을 제공한다.)

* 매개변수와 인자는 같은 의미로도 사용되긴 하지만 엄격히 따지면 다르다.


```javascript
function sum(){
    var i, _sum = 0;    
    for(i = 0; i < arguments.length; i++){
        document.write(i+' : '+arguments[i]+'<br />');
        _sum += arguments[i];
    }   
    return _sum;
}
document.write('result : ' + sum(1,2,3,4));
```


function `sum()` 에서 `()`안에 들어가는 게 매개변수 parameter,

`sum(1,2,3,4)` 에서 `()`안에서 함수로 전달되는 값을 인자 arguments.



## 매개변수의 수 


매개변수와 관련된 두가지 수가 있다.

- 함수.length : 함수.length는 함수에 정의된 인자의 수를 의미
- arguments.length : 함수로 전달된 실제 인자의 수를 의미


```javascript
function zero(){
    console.log(
        'zero.length', zero.length,
        'arguments', arguments.length
    );
}

function one(arg1){
    console.log(
        'one.length', one.length,
        'arguments', arguments.length
    );
}

function two(arg1, arg2){
    console.log(
        'two.length', two.length,
        'arguments', arguments.length
    );
}

zero(); // zero.length 0 arguments 0 
one('val1', 'val2');  // one.length 1 arguments 2 
two('val1');  // two.length 2 arguments 1
```
