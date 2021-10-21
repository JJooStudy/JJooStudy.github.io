---
layout: post
title:  "react Hook flow"
author: "JJoo"
comments: true
tags: react
---


## 훅의 호출 타이밍

react에서 setState는 이전값(prev)를 가져온다. setState가 알아서 받아온 이전값(prev)를 가지고 이전값과 반대의 값이 들어가게 간단하게 코드로 만들 수 있다. 

이때 useState에 변수말고 함수로 넣어주게 되면 조금 딜레이가 생겨서 로컬스토리지에서 꺼내거나, 작동까지 시간이 조금 필요할 때 활용할 수 있다. 


```javascript 
const [show, setShow] = React.useState(false);
function handleClick() {
  setShow((prev) => !prev);
  // 아래 if문을 react에서는 위처럼 간단하게 쓸 수 있다. 
  // if (show) {
  //   setShow(false);
  // } else {
  //   setShow(true);
  // }
}
```

## 훅의 호출 순서 

1. App의 render start 
2. useState 
3. App render end 
4. useEffect 선언 순서대로 … 

useEffect는 dependency array가 없든, 있든 선언한 순서대로 작동한다.


```javascript
import "./styles.css";
import React, { useEffect, useState } from "react";

export default function App() {
  console.log("App render start");

  const [show, setShow] = useState(() => {
    console.log("App useState");
    return false;
  });

  useEffect(() => {
    console.log("App Effect no deps");
  });
  useEffect(() => {
    console.log("App Effect empty deps");
  }, []);
  useEffect(() => {
    console.log("App Effect [show]");
  }, [show]);
  function handleClick() {
    setShow((prev) => !prev);
  }

  console.log("App render end");

  return (
    <>
      <button onClick={handleClick}>Search</button>
      {show && <Child />}
    </>
  );
}

// 자식 컴포넌트
const Child = () => {
  console.log("   Child render start");
  const [text, setText] = useState(() => {
    console.log("   Child useState");
    return "";
  });

  useEffect(() => {
    console.log("   Child Effect no deps");
  });
  useEffect(() => {
    console.log("   Child Effect empty deps");
  }, []);
  useEffect(() => {
    console.log("   Child Effect [text]");
  }, [text]);

  function handelChange(event) {
    setText(event.target.value);
  }

  const element = (
    <>
      <input onChange={handelChange} />
      <p>{text}</p>
    </>
  );
  console.log("   Child render end");
  return element;
};

```

결과 

- 버튼 누르기 전
![react hook flow 버튼 클릭 전 로딩순서](/images/img_react_hook_flow_1.png)


- 버튼 누른 후 
![react hook flow 버튼 클릭 후 로딩순서](/images/img_react_hook_flow_2.png)


#### 꼭 알아야 할 점

```App```이 그려지고 ```App```의 ```Child```가 그려지지만 

```useEffect```는 ```App```의 ```Child```의 ```useEffect```가 작동 후 ```App```의 ```useEffect```가 작동한다.


## useEffect의 clean up 

이전의 side effect를 지우고 다시 선언하는 것 


사용 방법 

```javascript
useEffect(() => {
  console.log("App Effect no deps");
  return () => {
    console.log("clean up");
  };
});

```

이 clean up은 ```useEffect```의 순서와 다르게 작동하는 데 부모의 Clean up이 먼저 작동한다.

clean up 예시 

```react
import "./styles.css";
import React, { useEffect, useState } from "react";

export default function App() {
  console.log("App render start");

  const [show, setShow] = useState(() => {
    console.log("App useState");
    return false;
  });

  useEffect(() => {
    console.log("App Effect no deps");
    return () => {
      console.log("App Effect no deps clean up");
    };
  });
  useEffect(() => {
    console.log("App Effect empty deps");
    return () => {
      console.log("App Effect empty deps clean up");
    };
  }, []);
  useEffect(() => {
    console.log("App Effect [show]");
    return () => {
      console.log("App Effect [show] clean up");
    };
  }, [show]);
  function handleClick() {
    setShow((prev) => !prev);
  }

  console.log("App render end");

  return (
    <>
      <button onClick={handleClick}>Search</button>
      {show && <Child />}
    </>
  );
}

// 자식 컴포넌트
const Child = () => {
  console.log("   Child render start");
  const [text, setText] = useState(() => {
    console.log("   Child useState");
    return "";
  });

  useEffect(() => {
    console.log("   Child Effect no deps");
    return () => {
      console.log("   Child Effect no deps clean up");
    };
  });
  useEffect(() => {
    console.log("   Child Effect empty deps");
    return () => {
      console.log("   Child Effect empty deps clean up");
    };
  }, []);
  useEffect(() => {
    console.log("   Child Effect [text]");
    return () => {
      console.log("   Child Effect [text] clean up");
    };
  }, [text]);

  function handelChange(event) {
    setText(event.target.value);
  }

  const element = (
    <>
      <input onChange={handelChange} />
      <p>{text}</p>
    </>
  );
  console.log("   Child render end");
  return element;
};

```

```useEffect```는 
	1. App의 Child의 ```useEffect```가 작동 후 
	2. App의 ```useEffect```가 작동 하는 데 

버튼을 클릭해보면 

![react hook flow 버튼 클릭 후 clean up 작동 순서](/images/img_react_hook_flow_cleanup_1.png)

순으로 작동한다.

	1. 부모 render
	2. 자식 render 
	3. 부모 Clean up 
	4. 자식 useEffect 
	5. 부모 useEffect 


여기서 Child에서 변화가 감지되어 ```useEffect```가 작동하게 되면 

![react hook flow 자식에 변화가 있을 때 작동 순서](/images/img_react_hook_flow_cleanup_2.png)

순으로 작동한다. 

	1. 자식 render 
	2. 자식 Clean up
	3. 자식 useEffect
  

또 여기서 버튼을 클릭해서 Child가 사라지면 

![react hook flow 자식에 변화가 있을 때 작동 순서](/images/img_react_hook_flow_cleanup_2.png)

순으로 작동한다.


	1. 다시 부모 render 
	2. 자식 Clean up
	3. 부모 Clean up
	4. 부모 useEffect 



이 순서를 잘 인지하고 있으면 side effect 발생 후 입력된 값을 지워주거나 초기값으로 다시 셋팅하거나 하는 작업을 원활하게 할 수 있다. 

