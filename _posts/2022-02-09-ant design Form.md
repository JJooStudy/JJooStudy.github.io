---
layout: post
title:  "ant design Form"
author: "JJoo"
comments: true
tags: antDesign
---

## ant design Form customizaion 하면서 생긴 문제점들

오늘은 ant design `Form`을 원하는 대로 customizing 하면서 생긴 문제들에 대해서 기록을 해보려고 한다. 

디자인 없이 프로젝트를 진행하는 상황에서 ui 라이브러리를 사용하기로 하면서 여러 유명한 ui 라이브러리 중 Material-ui는 사용해봐서 패스했고, 사용량이 많다는 Ant design을 pick해서 진행했다. 

Ant design이 디자인이 예쁘게 잘 잡혀있고, 다양한 컴포넌트를 제공하고, color나 스타일 가이드 제공이 많아보여서 customizing시 큰 문제가 생기지 않을 것 같았지만 .. 
역시나 여러 문제들이 발생했다 .. 부들... 

다른 컴포넌트들에서는 그래도 쉽게 해결을 했지만 `Form` 컴포넌트를 사용하면서는 꽤나 고생을 했기 때문에 기록을 남겨본다. 

### 구성방법

내가 프로젝트를 구성한 구조는 이러하다. 

```bash
...
├── components
│   └── common
│       └── input
│           ├── index.tsx
│           ├── input.tsx
│           ├── inputStyle.tsx
│           └── inputType.tsx
├── page
│   ├── pageName
│       └── index.tsx
...
``` 
구조에 대한 설명은 일단 넘어가도록 하고 

styled-component를 사용하여 `/components/common/input/inputStyle.tsx`에서 antd의 `Input`을 `import`해서 css 수정을 했고, `/page/pageName/index.tsx`에서 antd의 `Form`을 `import`해서 사용했다. 

#### 첫번째 문제 

customizing한 `Input`을 `Form`에서 사용하니 `onChange` event가 작동하지 않았다. 

다른 antd의 컴포넌트에서 사용한 customizing한 `Input`에서는 아무 문제 없이 잘 작동하던 기능이었는 데 
`Form`에서는 작동하지 않았고, 지정해놓은 type이 문제인가 싶어 여러 테스트를 해봤지만 해결점을 찾을 수 없었다. 

결국 `Form`안에서 `Input`의 `onChange` event를 사용하려면 antd의 `Input`을 사용해야 했다. 

#### 두번째 문제 

`Form`에 여러 형태의 raw를 보여줘야 했는 데 일일이 한줄한줄 넣어주니 코드가 너무 길어졌다. 그래서 `.map()`을 사용하면서 조건부 렌더링을 하기로 했다. 

``` 
const tableData = [
    {
      name: 'image_1',
      label: 'image_1_subtitle',
    },
    {
      name: 'image_2',
      label: 'image_2_subtitle',
    },
    {
      name: 'multi_input',
      label: 'multi_input_subtitle',
      extra: 'multi_input_desc',
    },
    {
      name: 'input_1',
      label: 'input_1_subtitle',
      extra: 'input_1_desc',
    },
    {
      name: 'input_2',
      label: 'input_2_subtitle',
      extra: 'input_2_desc',
    }
  ];
```
이렇게 `tableData`를 만들어서 

```react
{
  tableData.map((raw)=>{ 
    return (
    <Form.Item
      label={raw.label}
      name={raw.name}
      extra={raw.extra || null}
      key={raw.name}
    >
      {...}
    </Form.Item>
    )
  }) 
}
```

로 `Form raw`를 뿌려줬고 `{...}` 에는 `name`값에 따라 다른 포맷으로 표출되도록 조건부 렌더링 설정을 해주었다. 

```react
<Form
    ... 
    initialValues = {{
      image_1: 'image_1',
      image_2: 'image_2',
      multi_input: 'multi_input',
      input_1: placeData.address.jibeon,
      input_2: placeData.address.jibeon,
    }}
    ...
  >
    ...
  </Form.Item>
```

이렇게 `initialValues`을 설정해주었다. 

그런데 여기서 나타나는 문제! 

`name`이 `input_1`, `input_2`인 부분이 `initialValue`값이 들어가지 않는 것이었다. 

더 복잡한 포맷이었던 다른 `name`값에서도 `initialValue`값이 잘 들어가지는 데 단순한 포맷의 `input_1`, `input_2`에서만 들어가지 않았다. 

결국 반복하지 않아도 되었던 `<Form.Item></Form.Item>`을 조건부 렌더링 안으로 넣어주었다.

```
{
  tableData.map((raw)=>{ 
    return (
      <Fragment key={raw.name}>
        {
          조건1 && <Form.Item
            label={raw.label}
            name={raw.name}
            extra={raw.extra || null}
            key={raw.name}
          >
          ...
          </Form.Item>
        }
        
        {
          조건2 && <Form.Item
            label={raw.label}
            name={raw.name}
            extra={raw.extra || null}
            key={raw.name}
          >
          ...
          </Form.Item>
        }
      </Fragment>
    )
  }) 
}
```

아마도 조건부 렌더링이 되는 과정에서 `<Form.Item>`의 `name`값과 `<Input>`이 제대로 연결이 안되서 발생했던 것 같다. 
  
  
