---
layout: post
title:  "프로토타입(prototype)"
author: "JJoo"
comments: true
tags: Javascript
---

##  프로토타입이란?

프로토타입에는 3가지 종류가 있다. 

- prototype
- [[Prototype]]
    - `console.log`시 대괄호로 쌓여있는 프로퍼티
- constructor
    - 프로퍼티

생성자 함수가 있을 때 new 연산자로 인스턴스를 만들면 `constructor`의 `‘prototype’`이라고 하는 프로퍼티의 내용이 [[Prototype]]이라고 하는 프로퍼티로 참조를 전달하게 된다. 

즉 `constructor.prototype`과 `instance[[Prototype]]`이 같은 객체를 바라본다.

그런데 [[Prototype]]는 접근 가능한 것이 아니라 정보를 보여주기만 할 뿐으로 실제 동작상으로는 `instance`와 동일시가 된다.

![프로토타입의 구조](/images/prototpye_info.png)


## 배열에서의 protptype

배열`[ 1, 2, 3 ]`은 Array 생성자 함수와 그 프로토타입으로 이루어져 있는 데 `prototype`에는 배열과 관련한 메서드들이 모두 들어있다. 

`Array.prototype.constructor === Array()` 자기 자신을 가리킨다

`[ 1,2,3 ].constructor` 역시 `=== 배열의 생성자 함수 Array()`을 가리킨다 

`[ 1,2,3 ].constructor === [ 1,2,3 ].[[Prototype]].constructor === Array.prototype.constructor`

이렇게 모두 같다고 간주하게 되고 이 세가지는 모두 Array생성자 함수를 가리키게 된다. 

(실제로  `[ 1,2,3 ].[[Prototype]].constructor` 이렇게 접근이 가능한 건 아니고 개념이 같다는 것)

![배열의 프로토타입의 구조](/images/array_prototpye_info.png)

다시 정리하면 배열은 Array 생성자 함수와 그 프로토 타입으로 이뤄져 있고, prototype에는 배열과 관련된 메서드들이 모두 들어있다.

## 숫자 리터럴 

숫자 리터럴 자체는 객체가 아니므로 [[Prototype]] 프로퍼티가 있을 수 없다. 

그런데도 숫자 리터럴을 인스턴스인 것처럼 사용하려고 하면 (메서드를 쓰려고 하면) 자바스크립트가 임시로 숫자 리터럴에 해당하는 `Number` 생성자 함수의 인스턴스를 만들고 그 프로토타입에 있는 메서드를 적용해서 원하는 결과를 얻게 한 다음, 다시 그 인스턴스를 제거하는 방식으로 동작을 한다.

![숫자 리터럴의 프로토타입의 구조](/images/number_prototype_info.png)

## 문자열 리터럴

문자열 리터럴도 숫자 리터럴과 마찬가지로 어떤 메서드를 호출하는 순간에 임시로 문자열의 인스턴스를 만들어서 그 메서드를 실행하고 그 결과를 얻음과 동시에 인스턴스를 폐기한다.

![문자열 리터럴의 프로토타입의 구조](/images/string_prototype_info.png)

기본형 타입의 데이터들은 모두 이 같은 방식에 의해 메서드를 호출할 수 있게 된다. 

## 참조형 데이터

참조형 데이터들은 처음부터 인스턴스이기 때문에 메서드를 호출한 순간에 임시로 인스턴스를 생성했다가 폐기하는 복잡한 과정을 거칠 필요가 없다.

![참조형의 프로토타입의 구조](/images/function_prototype_info.png)


결론적으로 기본형 데이터이든 참조형 데이터이든, 메서드에 접근하고자 할땐 모두 위의 구조가 된다. 

데이터 자신에게는 메서드들이 없지만, 생성자함수의 `prototype` 프로퍼티에 있는 것을 [[Prototype]]이라는 연결통로에 의해서 마치 자신의 것처럼 쓸 수 있다

`null`과 `undefined`를 제외한 모든 데이터 타입은 이와 같은 생성자 함수가 존재하고, 각 생성자 함수의 프로토 타입에는 각 데이터 타입에만 해당하는 전용 메서드들이 정의가 되어 있다.

![프로토타입의 구조](/images/prototpye_info.png)

## instance로부터 prototype프로퍼티에 직접 접근할 수 있는 방법은 없을 까?

[[Prototype]]는 콘솔에서만 찍히는 내용일 뿐이고 이것을 이용해서 `prototype`에 접근할 수는 없다.

인스턴스로부터 `prototype`에 접근하는 방법에는 두가지가 있다.

- `instance.__proto__`
    - __proto__는 콘솔에는 보이지 않지만 접근이 가능하다.
    - 하지만 이 접근법은 ES2015에서 기존 브라우저들이 마음대로 제공하는 기능을 호환성 차원에서 할 수 없이 문서화 해준 것 뿐이기에 공식적인 방법을 쓰는 게 좋다.
- `Object.getPrototypeOf(instance)`
    - 공식방법

```javascript
function Person(n, a){
	this.name = a;
	this.age = a;
}

var laura = new Person('로라', 30);

var lauraClone1 = new laura.__proto__.constructor('로라_복제1', 10);

var lauraClone2 = new laura.constructor('로라_복제2', 25);

var lauraClone3 = new Object.getPrototypeOf(laura).constructor('로라_복제3', 20);

var lauraClone4 = new Person.prototype.constructor('로라_복제4', 15);
```

원본인 `laura`와 `클론1~4` 모두 `Person`의 인스턴스가 된다. 

왜냐하면 모두 동일한 프로퍼티( `Person.propotype` )에 접근 할 수 있기 때문이다. 

즉, 4가지 방법으로 생성자 함수의 prototype에 접근할 수 있다. 

- instance.__proto__
- instance
- Object.getPrototypeOf(instance)
- Constructor.prototype

또 아래 5가지 방식으로 생성자 함수에 접근할 수 있다는 말이 된다. 

- Constructor
- instance.__proto__.constructor
- instance.constructor
- Object.getPrototypeOf(instance).constructor
- Constructor.prototype.constructor

## 메소드  상속 및 동작 원리 

```javascript
function Person(n,a){
	this.name = n;
	this.age = a;
}

var laura = new Person('로라', 21);
var luna = new Person('루나', 20);

laura.setOlder = function(){
	this.age += 1;
}
laura.getAge = function(){
	return this.age;
}
luna.setOlder = function(){
	this.age += 1;
}
luna.getAge = function(){
	return this.age;
}
```

위 코드는 `Person()` 생성자로부터 `laura` 객체, `luna` 객체 두개의 인스턴스를 만들었다. 

그리고 `laura`와 `luna` 각각에 `setOrder`, `getAge`라는 메소드를 만들었다. 

인스턴스만 다를 뿐 내용과 기능이 똑같은 메소드가 반복된다. 이럴 때는 Person 생성자 함수에 메소드를 정의해서 상속시켜 반복을 줄일 수 있다.

```javascript
function Person(n,a){
	this.name = n;
	this.age = a;
}
Person.prototype.setOlder = function(){
	this.age += 1;
}
Person.prototype.getAge = function(){
	return this.age;
}
var laura = new Person('로라', 21);
var luna = new Person('루나', 20);
```

이렇게 `Person()` 생성자 함수의 `prototype`에 메소드를 정의하면 이전 인스턴트에 정의한 메소드와 똑같이 사용할 수 있다. 

`Person()` 생성자 함수의 `prototype`에 상속시켜서 사용하면 `Person`의 인스턴스를 계속 늘려도 해당 메소드에 대한 정의를 반복하지 않아도 자신의 메소드처럼 사용할 수 있다. 

이렇게 메모리 용량 최적화를 시킬 수 있다. 

객체지향적 관점에서 보면 어떤 집단(`Person`)의 특징(`property`)을 알 수 있는 좋은 수단이 되기도 한다.

## prototype chaining


`prototype` 프로퍼티는 객체이다. 

그말인즉 `prototype` 역시 `Object` 생성자 함수의 new 연산으로 생성된 인스턴스라는 말이 된다. 

따라서 **.prototype은 Object.prototype과 연결되어 있다.** 

즉, `instance`는 `constructor.prototype`와 연결되어 있는 `Object.prototype`에 있는 메소드도 자신의 메소드처럼 사용할 수 있다. 

이렇게 `prototype`에 연결되어 있는 빨간 화살표들[[ Prototype ]]을 **프로토타입 체인**이라고 부른다.

![프로토타입의 구조](/images/prototype_chaining.png)

> 프로토타입은 모두 객체이므로, 모든 데이터타입은 이와 동일한 구조를 따른다.
> 숫자형, 문자열, 배열, 함수 등 모두 Object.prototype과 프로토타입 체인으로 연결되어 있다.

`Object.prototype`에는 자바스크립트 전체를 통괄하는 공통된 메서드들이 정의되어 있다. 

- `hasOwnProperty()`
- `toString()`
- `valueOf()`
- `isPrototypeOf()`

이 `Object.prototype`의 메서드들은 모든 데이터 타입이 프로토타입 체이닝을 통해서 접근 할 수 있다. 

위와 같은 구조로 객체 프로토타입에 있는 메서드가 모든 데이터 타입에 적용되기 때문에 객체의 프로토타입에는 객체 전용 메서드를 정의해 둘 수 없다.

그래서 객체 전용 메서드는 객체 생성자 함수(`Object()`)에 직접 메서드를 정의하는 방법 밖에 없다. 

객체 관련 명령어들은 객체로부터 직접 메서드를 호출하는 대신 `Object.명령어`를 호출하면서 그 매개변수로 객체 자신을 넘겨주는 방식을 취하는 경우가 많다.

```javascript
// X
obj.freeze();
obj.keys();
obj.values();

// O
Object.freeze(obj);
Object.keys(obj);
Object.values(obj);

```

## 배열의 prototype

![배열의 프로토타입 체인 구조](/images/array_prototype_cain.png)

`Array.prototype`에는 `toString()` 메서드가 있다. 그래서 배열에 `toString()`을 호출하면 각 요소들을 콤마로 나열한 문자열이 출력된다. 

```javascript
[1,2,3].toString()
"1,2,3"
```

여기서 `Array.prototype`의 `toString()`을 삭제하면 출력이 다르게 나온다.

```javascript
delete Array.prototype.toString;

[1,2,3].toString();
// [object Array]
```

`Array.prototype`에서 `toString`을 삭제해도 prototype chaining으로 `Object.prototype.toString`을 상속해와서 사용하기 때문이다. 

여기서 `Object.prototype.toString`도 삭제하면 `toString` 메소드가 없다고 에러가 발생한다. 

인스턴스에서 메서드를 사용시 인스턴스 자기자신에서 먼저 메서드를 찾고 없으면 [[ Prototype ]]을 통해서 상속받는 값을 찾아가고 찾으면 실행하고 끝난다.



## Reference 
- 인프런 코어 자바스크립트 