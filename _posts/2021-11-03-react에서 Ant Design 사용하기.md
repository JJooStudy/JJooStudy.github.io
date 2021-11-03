---
layout: post
title:  "react에서 Ant Design 사용하기"
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




나머지는 차차 추가할 예정 

