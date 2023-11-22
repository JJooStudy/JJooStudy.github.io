---
layout: post
title:  "typescript에서 interface와 type의 차이"
author: "JJoo"
comments: true
tags: typescript
---

## typescript에서의 interface와 type

typescript에서 type을 정의할 때 type과 interface 두가지 방법이 있다. 

정의 방법은 아래와 같다.

```javascript
// type
type 타입명 = {
  id: string;
  label: string;
  onClick?: () => void;
}

// interface
interface 타입명 {
  id: string;
  label: string;
  onClick?: () => void;
}
```

type으로 선언하느냐, interface로 선언하느냐 차이가 조금 있는 것 빼고는 차이가 없어보인다. 

하지만 두 선언 방식에는 여러가지 차이가 있다. 

typescript를 공부하면서 차이점에 대해 공부를 하기는 했지만 사실 실사용에서는 항상 객체 타입으로 타입을 정의하고, 한번 타입을 선언해서 사용하고 나서 확장의 기능 등을 잘 사용하지 않았기에 큰 차이를 못 느끼고 있었는 데, 
이번에 디자인 시스템을 제작하면서 atomic한 component를 활용한 component를 제작하려 보니 type의 확장(상속)이 필요해지고 다시금 복습을 하면서 정리를 해본다. 


## interface와 type의 차이점 

### type은 다양한 종류의 타입을 정의할 수 있지만 interface는 객체타입만 정의할 수 있다. 

#### 원시타입

```javascript
// 가능
type stringType = string;
const stringData: stringType = 'abc';

type numberType = number;
const numberData: numberType = 123;

type booleanType = boolean;
const booleanData: booleanType = false;

// 불가능

interface stringInterface = string;
const stringData: stringInterface = 'abc';

```

#### 유니온 타입

```javascript
// 가능
type unionType =  string | any;
const stringData: unionType = 'abc';
const numberData: numberType = 123;

// 불가능
interface unionType =  string | any;
```

하지만 interface를 객체 차입을 선언하고 객체 타입 내부에서 유니온타입의 선언은 가능하다. 

```javascript
interface blablaType {
  id: string;
  // 가능
  label: string | any;
} 
```

type과 interface의 선언 방식을 보면 가능한지 가능하지 않은 지 알 수 있다. 

type은 ```=``` 기호를 사용해서 선언하고 interface는 `=`를 사용하지 않는 다.

그러니 `=` 사용해서 정의하는 원시타입의 선언이 불가능할 수 밖에(?)

### interface와 type의 확장(상속) 차이점 

```javascript
// type의 확장
type blaType = {
  id: string;
  label: string;
}

type blablaType = blaType & {
  onClick?: () => void;
}

// interface의 확장
interface blaInterface {
  id: string;
  label: string;
}

interface blablaInterface extands blaInterface {
  onClick?: () => void;
}
```

위 예시 code처럼 type의 확장은 `&`를 사용하고 interface의 확장은 `extends`를 사용한다. 

여기까지만 봐도 선언 방법 빼고는 확장도 큰 차이가 없다. 

하지만 가장 큰 차이점이 하나 있다. 

### interface의 선언적 확장 

type은 새로운 속성을 추가하기 위해서는 다른 이름으로 선언하여 기존 속성을 확장(상속) 시켜주면서 새로운 속성을 추가해야 하지만 

interface는 같은 이름으로 선언을 하면서 새로운 속성을 추가할 수가 있다. 

```javascript
// 불가능 
type blaType = {
  id: string;
  label: string;
}
type blaType = {
  onClick?: () => void;
}

// 가능
interface blaInterface {
  id: string;
  label: string;
}
interface blaInterface {
  onClick?: () => void;
}
```

interface는 같은 `blaInterface`으로 선언하면 `blaInterface`에 선언한 모든 타입이 담겨진다. 

## 그래서 interface와 type 중 뭐가 좋아? 

사실 interface와 type 중 뭐가 좋다고 할 순 없는 것 같다. 

필요한 기능에 따라서 상황에 맞게 type과 interface를 사용하면 될 것 같다. 

하지만 같이 일하는 협업 개발자들끼리는 코드 컨벤션처럼 규칙을 정하고 맞춰서 사용하는 게 혼돈을 주지 않을 듯 싶다. 

