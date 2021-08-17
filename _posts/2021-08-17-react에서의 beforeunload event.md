---
layout: post
title:  "beforeunload event"
author: "JJoo"
comments: true
tags: react
---


## react에서의 beforeunload event

[이전 게시글](https://jjoostudy.github.io/2021-08-13/beforeunload-event)에서 언급했던 것처럼 react에서 beforeunload event가 잘 작동되지 않는 경우가 있습니다.

react에서 beforeunload event 사용시 브라우저 닫기와 컴포넌트 안에서의 페이지 이동은 감지해내지만 

다른 컴포넌트를 사용한 페이지 이동과 뒤로가기 등은 감지하지 못합니다.

react의 원리가 virtual DOM을 이용하기 때문이라고 추측됩니다. 

그래서 react에서 다른 컴포넌트에서의 링크이동, 라우터 변경이 발생해도 beforeunload event가 작동하게 하려면 **react-use**라는 라이브러리를 사용합니다. 



[react-use](https://github.com/streamich/react-use/tree/90e72a5340460816e2159b2c461254661b00e1d3)

[react-use : useBeforeUnload](https://github.com/streamich/react-use/blob/HEAD/docs/useBeforeUnload.md)



## 설치

```javascript
npm i react-use 
```


## 사용방법 

```javascript
import React, { useState, useEffect } from 'react'
import { useRouter } from 'next/router'
import { useBeforeUnload } from "react-use";

const useBeforeUnloadContent = () => {

    const router = useRouter();
    const isConfirm = true;
    const message = "alert message";
  
    useBeforeUnload(isConfirm , message);
    
    useEffect(() => {
        const handler = () => {
          if (isConfirm && !window.confirm(message)) {
            throw "Route Canceled";
          }
        };
        router.events.on("routeChangeStart", handler);

      return () => {
        router.events.off("routeChangeStart", handler);
      }
    }, [isConfirm, message]);
    return (
      ...
    )
};
export default useBeforeUnloadContent;
```

새로고침시는 물론, 뒤로가기, gnb 메뉴 클릭하여 router 이동시에도, 다른 컴포넌트의 링크 이동시에도 감지하여 confirm창이 뜹니다.

confirm창에 지정한 문구가 뜨고 취소 버튼 클릭시 라우터 이동 X, 확인 버튼 클릭시 라우터 이동 O 

