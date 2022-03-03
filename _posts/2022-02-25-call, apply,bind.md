---
layout: post
title:  "call, apply, bind - this를 바인딩하는 방법"
author: "JJoo"
comments: true
tags: Javascript
---

##  명시적으로 this를 바인딩하는 방법 3가지

- call
- apply
- bind

## call, apply, bind의 API 문서

```javascript 
// 대괄호가 있으면 생략 가능하다는 뜻 
// 모두 thisArg가 필수 매개변수
func.call(thisArg[, arg1[, arg2[, ...]]])
func.apply(thisArg, [argsArray])
func.bind(thisArg[, arg1[, arg2[, ...]]])
```

### 예시 

```javascript 
function a(x, y, z){
	console.log(this, x, y, z);
}

var b = {
	bb: 'bbb'
};

a.call(b, 1, 2, 3);

a.apply(b, [1, 2, 3]);

var c = a.bind(b);

c(1,2,3);

var d = a.bind(b, 1, 2);

b(3);
```

`a.call(b, 1, 2, 3);`, `a.apply(b, [1, 2, 3]);`, `c(1,2,3);`, `b(3);` 모두 this로 `b()`를 바라보고 같은 결과를 호출한다.

결과 
```javascript 
{bb: “bbb”}  1 2 3 
```

`call`, `apply`와 `bind`의 차이는 `call`, `apply`는 바로 실행이 되고 `bind`는 함수에 설정값만 넣어주고 실행은 해당 함수를 실행해야 된다. 


## Reference 
- 인프런 코어 자바스크립트 