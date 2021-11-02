---
layout: post
title:  "styled-component와 AntDesign 함께 사용하기"
author: "JJoo"
comments: true
tags: react, antDesign
---


## styled-component와 AntDesign 함께 사용하기

#### AntDesign 설치 

```npm install antd```


#### 기본 셋팅 

전역으로 쓰일 AntDesign의 css(antd/dist/antd.css)는 styled-component로 만든 global-style.ts에 넣어줬다. 

[styled-component 셋팅하기](https://jjoostudy.github.io/2021-10-29/Next.js%EC%97%90-styled-components-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

```react
import { createGlobalStyle } from 'styled-components';
import { reset } from 'styled-reset';
import 'antd/dist/antd.css';

export const GlobalStyle = createGlobalStyle`
  ${reset}
  /* CSS 초기화 */
  ... 
`;

```


AntDesign으로 레이아웃부터 작은 컴포넌트까지 모두 활용해서 사용해보려고 셋팅을 하다보니 css 커스텀이 필요했다.

일반 css로 설정을 하지 않고 AntDesign을 styled-component로 오버라이딩을 해서 사용해주면 쉽게 커스텀을 할 수 있다. 

styled-component의 ```styled()``` 사용 

```react
import React from 'react';
// ant design
import { Layout } from 'antd';
const { Sider } = Layout;
// styled-component
import styled from 'styled-components';

function Siders() {
  return <SiderStyle style={{ color: '#fff' }}>sider</SiderStyle>;
}
export default Siders;

const SiderStyle = styled(Sider)`
  background-color: #fff;
`;

```




