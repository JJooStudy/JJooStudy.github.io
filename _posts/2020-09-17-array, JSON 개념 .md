---
layout: post
title:  "array, JSON 개념"
author: "JJoo"
comments: true
tags: Javascript
---


* array : 값의 배열 [] 대괄호 사용 
* JSON : 자바스크립트 객체 표현식
* object : 자바스크립트 객체 
* string : 텍스트(반드시 따옴표 사용)
* boolean : true or false



## 1. array 배열 

: 순서index가 있는 값value의 나열


```javascript
let number = [4, 8, 6, 18]

// 변수 number는 0~3까지 4개의 인덱스를 가지고 있고
// 각 인덱스의 값은 아래와 같이 된다.

index = value
index 0 = 4
index 1 = 8
index 2 = 6
index 3 = 18

console.log(number) // ▶ (4) [4, 8, 6, 18]
                       0: 4
                       1: 8
                       2: 6
                       3: 18
                       length: 4
                       __proto__: Array(0)
                   // -> (4)는 인덱스바 : 인덱스의 수.
                   // [4, 8, 6, 18] 변수의 값 : 전체 요소가 나온다.
                   // ▶를 눌러 자세한 정보를 볼 수 있다.
```


: 배열에서 할 수 있는 일


1. 값 찾기 : 해당 인덱스의 값이 나온다.

array[index]


```javascript
number[1] //8
```


2. 값 변경 : 해당 인덱스의 값을 변경 할 수 있다.

array[index] = value


```javascript
number[1] = 7
number // ▶ (4) [4, 7, 6, 18]
```


3. 배열의 길이를 알아낼 수 있다. .length

array.length

```javascript
number.length //4
```


4. 배열의 끝에 요소를 추가할 수 있다.

array.push() -> 숫자, 문자열, 배열, 객체 등등 들어갈 수 있다.

```javascript
number.push(20, 30)
number // ▶ (6) [4, 7, 6, 18, 20, 30]
```


5. 배열의 끝에 요소를 삭제할 수 있다.

array.pop() -> 괄호 안은 공란

```javascript
number.pop()
number // ▶ (5) [4, 7, 6, 18, 20]
```


## 2. JSON

: JavaScript Object Notation  자바스크립트에서 객체를 만들때 사용하는 표현식

: 클라이언트와 서버간의 데이터를 주고 받을 때 사용할 수 있는 표현의 하나 

: 객체를 {} 대괄호로 표현. 


```javascript
{ "key값" : "Data(value)" }
```


: 객체간 구별은 , 로 구분 


```javascript
{ "key값" : "Data(value)" }, { "key값" : "Data(value)" }, { "key값" : "Data(value)" }
```

: 객체 내부 key 분류도 , 로 구분

```javascript
{ "key값" : "Data(value)",  "key값" : "Data(value)", "key값" : "Data(value)" }
```

: Json데이터의 자료형은 Hashmap, array ~ value(String, int , double …)까지 다양하게 표현이 가능

[http://www.json.org/json-ko.html](http://www.json.org/json-ko.html) 참고


: JSON 데이터 형식은 아래와 같다.

```javascript
JsonObject => { {}, {}, [{}, {}, {}] }

JsonArray => [{}, {}, {}]

JsonElement => {}  

// 마치 JsonObject 와 같은 것이 되어버림. 
// 왜냐면 JsonObject 는 모든 데이터 형식을 가질 수 있기 때문에 
// {} 하나만으로 이루어진 데이터도 JsonObject가 될 수 있다

```



JSON.parse(), JSON.stringify()
