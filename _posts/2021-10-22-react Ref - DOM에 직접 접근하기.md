---
layout: post
title:  "react Ref - DOM에 직접 접근하기"
author: "JJoo"
comments: true
tags: react
---

## Ref

Ref란 render 메소드에서 생성된 DOM노드나 React 앨리먼트에 접근하는 방법을 제공합니다.

```props```로 변경하는 일반적인 date flow가 아닌 직접적으로 자식을 변경해야 하는 경우가 있는 데, 여기서 자식은 컴포넌트의 인스턴스일 수도 있고, DOM 엘리먼트일 수도 있다.

변경하고자 하는 자식에게 이름을 달아주는 것이 ref다.

```document.getElementById```보다 리액트에 최적화되어 사용하기 좋다.

Ref 변수로 지정한 객체의 ```current``` 속성을 통해서 접근할 수 있다. 


#### ref 사용 사례

- ```input``` / ```textarea``` 등에 포커스를 해야 할때 혹은 ```media``` 재생을 관리할 때
- 특정 DOM 의 크기를 가져와야 할 때
- 특정 DOM 에서 스크롤 위치를 가져오거나 설정을 해야 할 때
- 외부 라이브러리 (플레이어, 차트, 캐로절 등) 을 사용 할 때
- 애니메이션을 직접적으로 실행시킬 때 


## 사용방법

class component에서는 react의 내장함수인 ```createRef```, functional component에서는 hook인 ```useRef()```를 사용한다.


## class component - createRef

#### ref 생성하기

```react
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

Ref는 ```React.createRef()```를 통해서 생성되고 ref 속성을 통해 react 엘리먼트에 연결된다. 

보통 컴포넌트의 인스턴스가 생성될 때 ref를 프로퍼티로서 추가하고, 그럼으로서 컴포넌트의 인스턴스의 어느 곳에서도 ref에 접근할 수 있다. 


#### ref 접근하기

```
const node = this.myRef.current;
```

생성된 ref는 그 노드를 향한 참조를 ```current``` 속성에 담는다. 

ref속성이 HTML 엘리먼트에 쓰였다면, 생성자에서 ```React.createRef()```로 생성된 ref는 자신을 전달받은 DOM 엘리먼트를 ```current``` 프로퍼티의 값으로서 받는다.

ref속성이 custom class component에 쓰였다면, ref 객체는 마운트된 컴포넌트의 인스턴스를 ```current``` 프로퍼티의 값으로서 받습니다.

functional component는 인스턴스가 없기때문에 함수컴포넌트에 ref속성을 사용할 수 없다. 


예시
```react
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // 이 코드는 동작하지 않습니다.
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}

```

하지만 DOM 엘리먼트나 class component의 인스턴스에 접근하기 위해 ref속성을 functional component에 사용하는 것은 된다. 


## functional component - useRef()

```react
function CustomTextInput(props) {
  // textInput은 ref 어트리뷰트를 통해 전달되기 위해서
  // 이곳에서 정의되어야만 합니다.
  const textInput = useRef(null);

  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} 
      />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

```useRef()``` hook을 사용하여 ```inputRef``` 객체를 생성한 후, ```<input>``` 엘리먼트의 ref prop에 넘긴다. 

이렇게 해주면 ```inputRef``` 객체의 ```current``` 속성에는 ```<input>``` 엘리먼트의 레퍼런스가 저장됩니다. 

따라서 ```<button>``` 엘리먼트의 click 이벤트 핸들러에서는 ```inputRef.current```로 ```<input>``` 엘리먼트를 제어할 수 있다.




