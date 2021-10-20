---
layout: post
title:  "react custom Hook"
author: "JJoo"
comments: true
tags: react
---

## Custom Hooks 

[https://ko.reactjs.org/docs/hooks-custom.html](https://ko.reactjs.org/docs/hooks-custom.html)


```useState```와 ```useEffect```를 반복하여 사용할때 Hook 로직을 묶어서 재사용하도록 공통으로 만드는 것을 custom hook이라고 한다.

hook에서 hook으로 정보 전달이 가능하다. 


```javascript
useEffect( () => {
	window.localStorage.setItem("keyword", keyword);
	window.localStorage.setItem("result", result);
}, [ keyword, result ]);
```

```useEffect```의 두번째 인자에 배열은 ```useEffect```를 실행시키는 조건이라 위의 예시처럼 ```useEffect```의 두번째 인자에 배열로 여러가지를 넣게 되면 ```keyword```가 변경될때 ```window.localStorage.setItem("result", result);```도 같이 작동하게 되는 불필요한 작동이 생기게 된다.

이를 해결하려면 ```useEffect```를 각각 개별로 작성해야 ```keyword```이 변경될때, ```result```가 변경될때 각각 개별로 작동하게 된다.

```javascript
useEffect( () => {
	window.localStorage.setItem("keyword", keyword);
}, [ keyword ]);

useEffect( () => {
	window.localStorage.setItem("result", result);
}, [ result ]);
```


이렇게 반복되는 코드를 custom hook으로 작성할 수 있다. 

custom hook은 꼭 **use로 시작해야** 특정 함수가 그안에서 hook을 호출하는 지 알 수 있고 Hook 규칙의 위반 여부를 자동으로 체크할 수 있다. 


```javascript
function useLocalStorage( itemName ){
	const [ state, setState ] = React.useState( () => {
		return window.loacalStorage.getItem( itemName ) || "";
	} )
	useEffect( () => {
		window.localStorage.setItem( itemName , state);
	}, [ state ]);
	
	return [ state, setState ];
}

const App = () => {
	const [ keyword , setKeyword ] = useLocalStorage( "keyword" );
	cosnt [ result , setResult ] = useLocalStorage( "result" )
}
```


default 값을 다른 타입으로 지정하고 싶다면 

```javascript
function useLocalStorage( itemName , value = "" ){
	const [ state, setState ] = React.useState( () => {
		return window.loacalStorage.getItem( itemName ) || value ;
	} )
	useEffect( () => {
		window.localStorage.setItem( itemName , state);
	}, [ state ]);
	
	return [ state, setState ];
}

const App = () => {
	const [ keyword , setKeyword ] = useLocalStorage( "keyword" );
	cosnt [ result , setResult ] = useLocalStorage( "result" );
	cosnt [ typing , setTyping ] = useLocalStorage( "typing" , false );
}
```

두번째 인자로 ```value```값을 넘겨줄때는 넘겨준 ```value```값이 들어가고 두번째 인자가 들어가지 않을 때는 ```value=""```로 지정한 default값인 ```""``` 이 적용된다.  

