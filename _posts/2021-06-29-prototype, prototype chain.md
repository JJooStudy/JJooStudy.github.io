---
layout: post
title:  "prototype, prototype chain"
author: "JJoo"
comments: true
tags: Javascript
---


## 상속시킬때 생성자로 만드는 이유 .

일단 생성자는 함수. 

함수를 호출할때 `new`를 붙이면 생성자가 되고 그 결과는 새로운 객체를 만들어서 그 객체를 리턴하기 때문에 생성자를 호출하는 변수는 새로운 객체를 담게 된다. 

객체 리터럴 `var a = {};`로 만들지 않고 생성자로 만드는 이유는 객체를 만들때 그 객체가 가지고 있어야 하는 데이터, 메소드 등을 기본적으로 가지고 있게 하려고다.



## prototype이란?

말 그대로 객체의 원형이다.

함수는 객체이고 생성자로 사용될 함수도 객체이다.

객체는 프로퍼티를 가질 수 있는 데 prototype이라는 프로퍼티는 그 용도가 약속되어 있는 특수한 프로퍼티다. 

prototype에 저장된 속성들은 생성자를 통해서 객체가 만들어질때마다 그 객체에 연결이 된다.


```javascript
function Fruit(){}
Fruit.prototype.fruitProp = true;

function apple (){}
apple.prototype = new Fruit();

function redApples (){}
redApples.prototype = new apple();

const redAppleOne = new redApples();

console.log(redAppleOne.fruitProp);
```


결과는
`true`

![prototype 구조](/images/ex_prototype.png "prototype 구조")



## prototype chain


prototype으로 서로 연결되어 있는 것을 prototype chain이라고 한다.

`redAppleOne`에서 `fruit`의 프로퍼티 `fruitProp`에 접근할 수 있는 이유는 prototype chain으로 `redAppleOne`와 `fruit`이 **연결**되어 있기 때문이다.

```javascript
function Fruit(){}
Fruit.prototype.fruitProp = true;

function apple (){}
apple.prototype = new Fruit();

function redApples (){}
redApples.prototype = new apple();

const redAppleOne = new redApples();
console.log(redAppleOne.fruitProp);
```


위 코드는 아래와 같이 작동한다.


1. redAppleOne에서 fruitProp를 찾는다.
2. 없다면 redApples.prototype에서 찾는다.
3. 없다면 apple.prototype에서 찾는다.
4. 없다면 Fruit.prototype에서 찾는다.


이렇게 연결된 가까운 부모 순으로 찾아간다.


![prototype chain 구조](/images/ex_prototype_chain.png "prototype chain 구조")


### 주의할 점

`apple.prototype = new Fruit();` 이렇게 생성자로 연결하는 것이 아닌 `apple.prototype = Fruit.prototype;`로 연결하면 `apple.prototype`이 변경됐을 때 `Fruit.prototype`에도 영향을 주게 된다.

이렇게 `apple.prototype = new Fruit();` 생성자로 만들어야 상속하고자 하는 부모를 원형으로 하는 새로운 객체를 생성하기 때문에 `apple.prototype`이 변경되어도 `Fruit.prototype`에 영향을 주지 않는다. 

