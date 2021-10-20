---
layout: post
title:  "react Hooks"
author: "JJoo"
comments: true
tags: react
---

## React Hook

class component에서는 컴포넌트간에 상태로직을 재사용하기 힘들었다. 

HOOK은 class component의 단점을 보완하면서 LifeCycle 등과 관련된 함수를 재사용이 가능하도록 한다. 

react 16.8부터 추가됐다.

useState, useEffect를 사용하여 상태관리를 한다.

계층의 변화 없이 상태관련 로직을 재사용할 수 있게 해준다. 

[https://ko.reactjs.org/docs/hooks-intro.html](https://ko.reactjs.org/docs/hooks-intro.html)



## hook overview 

[https://ko.reactjs.org/docs/hooks-overview.html](https://ko.reactjs.org/docs/hooks-overview.html)


## Hook 사용규칙

- **최상위에서만** hook을 호출해야 한다. (반복문, 조건문, 중첩된 함수에서 사용X)
- functional component와 custom hook내에서만 호출해야 한다. 


## State Hook 

[https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

```javascript
importReact,{ useState } from 'react';

function Example(){ 
	const [count,setCount] = useState(0);
	return(
	<div> 
	        <p>You clicked {count} times</p> 
	        <buttononClick={()=>setCount(count +1)}>
		         Click me
	        </button>
	      </div>
	);
}
```

- ```importReact,{useState }from'react'; ```: useState Hook을 React에서 가져온다.
- ```const [count, setCount] = useState(0);``` : useState Hook을 이용하면 state 변수와 해당 state를 갱신할 수 있는 함수가 만들어집니다. 또한, useState의 인자의 값으로 ```0```을 넘겨주면 ```count```값을 ```0```으로 초기화할 수 있습니다.
- ```<buttononClick={()=>setCount(count +1)}>``` : 사용자가 버튼 클릭을 하면 ```setCount``` 함수를 호출하여 state 변수를 갱신합니다. React는 새로운 ```count``` 변수를 ```Example``` 컴포넌트에 넘기며 해당 컴포넌트를 리렌더링합니다.


## Effect hook 

[https://ko.reactjs.org/docs/hooks-effect.html](https://ko.reactjs.org/docs/hooks-effect.html)

Effect hook을 사용하면 함수 컴포넌트에서  side effect(데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것 등)를 수행할 수 있다. 

class  component의 ```componentDidMount```, ```componentDidIUpdate```, ```componentWillUnmount```가 합쳐진 것으로 생각할 수 있다. 

class component에서 ```componentDidMount```와 ```componentDidUpdate```에서 중복으로 코드를 작성해야 했던 경우가 있는 데 functional component에서는 ```useEffect(()=>{});```로 한번에 작성할 수 있다.


#### class component 에서 사용할 때 

```javascript
// 페이지가 마운트될때 구독
componentDidMount() {
  ChatAPI.subscribeToFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}
// 언마운트될때 구독해지 
componentWillUnmount() {
  ChatAPI.unsubscribeFromFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}
handleStatusChange(status) {
  this.setState({
    isOnline: status.isOnline
  });
}
```


#### Hook을 사용할 때 

```javascript
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // 마운트될때 구독 
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  // effect 이후 clean-up 에서 구독해지 
  return function cleanup() { // arrow function으로 작성해도 됌 
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```


## Effect가 업데이트 시마다 실행되는 이유

class component에서는 업데이트가 될때마다 계속 업데이트를 잡아주는 작업을 해줘야하는 번거로움과 업데이트 코드를 빼먹으면 버그가 발생했다(수동적). 

```useEffect```는 기본적으로 업데이트를 계속 다루기때문에 업데이트를 위한 특별한 코드를 추가하지 않아도 되고 버그를 방지해준다.  

```useEffect```에 dependency array를 사용하면 원하는 부분의 변화만 캐치하여 ```useEffect```를 작동 시킬 수 있다. 


ex)
```useEffect(()=>{},[dependency array]}```

class component 에서는 ```componentDidUpdate```에서 값을 전달받아서 그 값이 이전값과 달라졌을 때 업데이트 되게 했음  

```
componentDidUpdate(prevProps, prevState){
  ...
}
```


## Custom Hooks 

[https://ko.reactjs.org/docs/hooks-custom.html](https://ko.reactjs.org/docs/hooks-custom.html)


컴포넌트들에서 중복되는 Hook 로직을 묶어서 재사용하도록 한다. 

hook에서 hook으로 정보 전달이 가능하다. 


#### 본래의 예시와 작동 방식에 어떤 변화도 없이 정확히 같은 방식으로 작동한다. 

오로지 공통의 코드를 뽑아내 새로운 함수로 만든 것뿐이다.

사용자 정의 Hook은 React의 특별한 기능이라기보다 기본적으로 Hook의 디자인을 따르는 관습이다.


#### 사용자 정의 Hook의 이름은 꼭 “use”로 시작되어야 한다. 

이를 따르지 않으면 특정한 함수가 그 안에서 Hook을 호출하는지를 알 수 없기 때문에 Hook 규칙의 위반 여부를 자동으로 체크할 수 없다.


#### 같은 Hook을 사용하는 두 개의 컴포넌트는 state를 공유하지 않는다.

사용자 정의 Hook은 상태 관련 로직(구독을 설정하고 현재 변숫값을 기억하는 것)을 재사용하는 메커니즘이지만 사용자 Hook을 사용할 때마다 그 안의 state와 effect는 완전히 독립적이다.


#### 사용자 정의 Hook, 각각의 Hook에 대한 호출은 서로 독립된 state를 받는다.

 use'CustomHook'을 직접적으로 호출하면 React의 관점에서 이 컴포넌트는 ```useState```와 ```useEffect```를 호출한 것과 다름없고, 하나의 컴포넌트 안에서 ```useState```와 ```useEffect```를 여러 번 부를 수 있기에 이들은 모두 완전히 독립적입니다.


## 추가 hooks 

hook api 참고서 
[https://ko.reactjs.org/docs/hooks-reference.html](https://ko.reactjs.org/docs/hooks-reference.html)


#### 기본 Hook

- useState
- useEffect
- useContext


#### 추가 Hooks

- useReducer
- useCallback
- useMemo
- useRef
- useImperativeHandle
- useLayoutEffect
- useDebugValue


**useState** : 이전 값을 인자로 / 초기화 지연(함수)

**useEffect** : 의존성 배열, 안주거나 / ```[]``` / ```[a, b]```

**useLayoutEffect** : ```useEffect``` 와 유사 모든 DOM 변경 후 브라우저가 화면을 그리기 이전 시점에 동기적으로 수행됨

**useReducer** : ```useState``` 대체 state / reducer / action



#### useReducer 사용 예시 


```javascript
import React, { useReducer } from "react";

export default function HooksReducer() {
  const initialState = { count: 0 };
  
  function reducer(state, action) {
    switch (action.type) {
      case "reset":
        return initialState;
      case "increment":
        return { count: state.count + 1 };
      case "decrement":
        return { count: state.count - 1 };
      default:
        throw new Error();
    }
  }
  
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <div>
      Count : {state.count}
      <button onClick={() => dispatch({ type: "reset" })}>reset</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </div>
  );
}
```

- state와 dispatch라는 함수를 리턴해준다
- ```useReducer```에 초기값만 넣어주는 것이 아니라 ```reducer```도 넣어준다.
- ```reducer```는 사실상 함수.
- ```onClick```에서 ```setState```가 아닌 dispatch로 넣어준다.
- 굳이 ```useState```가 아닌 ```useReducer```로 작업하는 이유는 동작에 따른 name값 변경이라던지, 여러 case 별 다른 동작 등 다양한 상태관리가 필요한, 좀더 복잡한 기능이 요구될때 간편해진다. 



**useContext** : Context

**useCallback & useMemo** : 메모이제이션 

**useRef** : current 라는 상자. 내용의 변경은 알려주지 않음. 콜백 ref 사용



## 요약

- Hooks는 class component의 단점을 보완하고 재사용성을 강화한다.
- Hook은 functional component와 custom hook에서만 사용해야 하고 최상위에 위치해야 한다.
- useEffect는 데이터 fetch, 구독 설정하기, 수동으로 DOM을 수정하는 등 side effect(부수효과)를 수행한다.
- useEffect의 clean up은 구독과 구독해지(이전의 side effect를 지우고 다시 선언)를 한공간에서 처리해준다.
- useEffect의 dependency array는 필요한 변경시에만 effect가 실행되게 한다.
- 반복되는 hooks를 공통으로 묶어서 관리하는 것을 custom hook이라고 한다.


