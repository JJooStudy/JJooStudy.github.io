---
layout: post
title:  "react 합성(Composition)"
author: "JJoo"
comments: true
tags: react
---


## react에서 합성(Composition)이란?

아주 간단히 말하면 component안에서 컴포넌트를 모아서 출력하는 것.

컴포넌트를 설계할때 고려해야할 부분들이 있다. 

=== 컴포넌트에 다른 컴포넌트를 담기


## 합성 (Composition) vs 상속 (Inheritance)

[react composition 공식문서 살펴보기](https://ko.reactjs.org/docs/composition-vs-inheritance.html)

담는 방법에는 children으로 받아오는 방식과 custom component에 세부내용만 받아오는 방식이 있다.


#### children으로 받아오는 방식

WelcomeDialog.jsx (부모)
```react
import React from "react";
import CompositionDialog from "./CompositionDialog";
import CustomDialog from "./CustomDialog";
export default function WelcomDialog() {
  return (
    <>
      <CompositionDialog>
        <h1>Welcome</h1>
        <h5>Thank you</h5>
      </CompositionDialog>
      <CustomDialog title="Welcome" description="thank you" />
    </>
  );
}
```


CompositionDialog.jsx
```react
import React from "react";
export default function CompositionDialog(props) {
  return <div style={{ backgroundColor: "pink" }}>{props.children}</div>;
}
```

감싸고 있는 div에 스타일만 주고 내용이 되는 children은 부모에서 받아오는 구조 

어떤 자식 컴포넌트가 들어올지 미리 예상할 수 없는 경우에 사용한다. 



#### custom component에 세부내용만 받아오는 방식

WelcomeDialog.jsx (부모)
```react
import React from "react";
import CompositionDialog from "./CompositionDialog";
import CustomDialog from "./CustomDialog";
export default function WelcomDialog() {
  return (
    <>
      <CompositionDialog 
        title="title"
        description="description"
      />
      <CustomDialog title="Welcome" description="thank you" />
    </>
  );
}
```

CustomDialog.jsx
```react
import React from "react";
export default function CustomDialog(props) {
  return (
    <div>
      <h1>{props.title}</h1>
      <h5>{props.description}</h5>
    </div>
  );
}
```



```props```로 세부 내용만 받아오고 컴포넌트의 구조(틀)은 모두 자식 컴포넌트에서 만들어서 리턴해주는 구조 



## 컴포넌트의 합성의 특수화 specialization 


일반적인 컴포넌트를 기준으로 더 구체적인 컴포넌트를 구현하는 것.


ex) ```Dialog``` 컴포넌트를 기준으로  ```WelcomeDialog``` 컴포넌트를 만드는 것 

부모인 ```App``` 컴포넌트가 변경이 되어도 ```WelcomeDialog```에 대한 불필요한 리렌더링을 막을 수 있다. 

```react 
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}

function App() {
  return (
    <WelcomeDialog />
  );
}
```



## 컴포넌트의 확장 

다양한 상황을 품을 수 있도록 해준다. 


```react 
import React, { useState } from "react";
export default function Dialog(props) {
  const [isOpen, setIsOpen] = useState(false);
  return (
    <>
      <button onClick={() => setIsOpen(true)}>Open</button>
      {isOpen && (
        <div
          style={{
            position: "absolute",
            zIndex: 99,
            top: "50%",
            left: "50%",
            transform: "translate(-50%, -50%)",
            border: "1px solid black",
            padding: 24,
            backgroundColor: "white",
          }}
        >
          {/* 타입이 스트링이면, 스트링이 아니면  */}
          {typeof props.title === "string" ? (
            <h1>{props.title}</h1>
          ) : (
            props.title
          )}
          <h5>{props.description}</h5>
          <button
            style={{ backgroundColor: "red", color: "white" }}
            onClick={() => setIsOpen(false)}
          >
            {props.button}
          </button>
        </div>
      )}
      {isOpen && (
        <div
          style={{
            position: "fixed",
            top: 0,
            left: 0,
            backgroundColor: "lightgray",
            width: "100%",
            height: "100%",
          }}
        />
      )}
    </>
  );
}
```

```props```의 타입에 따라서 다르게 적용할 수 있다. 

```typeof props.title``` : props.title의 type값을 받아옴


```react 
{typeof props.title === "string" ? (
  <h1>{props.title}</h1>
) : (
  props.title
)}
```

자식을 부르는 부모에서 보내는 ```props```의 ```title="welcome"```이면 ```string```이므로 
```<h1>{props.title}</h1>```을 리턴하고 ```title={<h1 style={{ color: "puple" }}>Thanks</h1>}```이면 
```string```이 아니므로 ```props.title```인 ```<h1 style={{ color: "puple" }}>Thanks</h1>```를 리턴한다. 












