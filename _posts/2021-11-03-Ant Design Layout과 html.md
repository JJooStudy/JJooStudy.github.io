---
layout: post
title:  "Ant Design Layout과 html"
author: "JJoo"
comments: true
tags: antDesign
---


## Ant Design Layout

Ant Design은 Layout을 불러오고 Layout에서 다시 필요한 부분을 선언하는 사용하는 방식으로 되어 있다. 

ex) 
```react 
import { Layout } from 'antd';
const { Header, Footer, Content } = Layout;
```

Ant Design Layout의 각 컴포넌트들이 어떤 html 태그로 만들어지는 지 살펴보자. 


#### Ant Design Layout - Header 

```react
import { Layout } from 'antd';
const { Header } = Layout;
```

html `<header></header>`


#### Ant Design Layout - Footer

```react
import { Layout } from 'antd';
const { Footer } = Layout;
```

html `<footer></footer>`


#### Ant Design Layout - Layout

```react
import { Layout } from 'antd';
```

html `<section></section>`


#### Ant Design Layout - Content

```react
import { Layout } from 'antd';
const { Content } = Layout;
```

html `<main></main>`


#### Ant Design Layout - Sider

```react
import { Layout } from 'antd';
const { Sider } = Layout;
```

html `<aside></aside>`


#### Ant Design Typography - Title

```react
import { Typography } from 'antd';
const { Title } = Typography;
...

<Title>h1</Title>
<Title level={2}>h2</Title>
<Title level={3}>h3</Title>
<Title level={4}>h4</Title>
<Title level={5}>h5</Title>

```

html
```html
  <h1>h1</h1>
  <h2>h2</h2>
  <h3>h3</h3>
  <h4>h4</h4>
  <h5>h5</h5>
```


#### Ant Design Typography - Text

```react
import { Typography } from 'antd';
const { Text } = Typography;
```

html `<span></span>`

antd의 `<Space>`로 감싸진 `<Text>`는 `<div><span></span></div>`의 형태로 그려지고, 

`<Space>`로 감싸지지 않은 `<Text>`는 `<span></span>`으로 그려진다. 

필요에 따라 `<Text as="p"></Text>`로 사용하면 편하다. 



또 특이한 부분이 있으면 추가할 예정 

