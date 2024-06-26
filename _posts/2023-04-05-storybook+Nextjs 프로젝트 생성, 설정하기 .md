---
layout: post
title:  "storybook+Nextjs 프로젝트 생성, 설정하기"
author: "JJoo"
comments: true
tags: storybook next.js
---

# storybook이란? 

storybook이란 ui component 개발 도구로 
ui component를 분리된 환경에서 체계적이고 효율적으로 개발할 수 있고 
간편하게 문서화 시켜주기 때문에 관리에 용이하다.

또 독립된 환경에서 컴포넌트별 테스트가 용이하다. 

주로 디자인 시스템 개발 혹은 사내 ui 라이브러리 문서로 사용된다고 한다. 

필자는 이번에 디자인 시스템을 개발하면서 storybook을 접하게 되었다. 

storybook은 스토리라는 컴포넌트 미리보기를 작성해두면 화면에서 디자인과 코드 등의 결과물을 바로 확인할 수 있기 때문에 디자이너와 개발자의 협업에 매우 용이하다.

또 다양한 웹 프레임워크(react, vue, angular) 환경을 지원하고 있다. 


# Next.js에 storybook 셋팅하기 

필자는 거의 Next.js 프로젝트를 구축/관리하고 있기 때문에 Nextjs에 storybook을 셋팅하기로 했다. 


## 1. Next.js 프로젝트 생성하기. 

일단 프로젝트를 생성하고 해당 프로젝트에 스토리북을 셋팅하는 순서이다. 

```
npx create-next-app [프로젝트명]
```

으로 새 Nextjs 프로젝트를 생성한다. 


## 생성된 Next.js 프로젝트에 storybook 셋팅하기. 

Nextjs + storybook을 사용하려면 webpack5로 빌드가 되도록 설정이 되어야 한다. 

```
npx sb init --builder webpack5
```

``` npx storybook@next init ```로 storybook 설정 후 빌드 설정을 수정할 수 있겠지만 
storybook 셋팅하면서 빌더도 설정하는 게 편하다. 

또 node 버전이 낮다면 여기서 버전 문제로 storybook이 설치가 안되는 문제를 마주할 수 있다. 

노드 버전을 업그레이드 하거나 nvm으로 노드 버전을 관리해서 해결할 수 있다. 

여기서 nvm사용시 한글 폴더명때문에 경로 오류 문제가 있었으나 [nvm root 재설정](https://jjoostudy.github.io/2023-04-04/nvm-%EA%B2%BD%EB%A1%9C-%EC%98%A4%EB%A5%98-C-Users-%D6%BF-AppData-Roaming-nvm-could-not-be-found-or-does-not-exist-%ED%95%B4%EA%B2%B0)으로 해결!

## storybook 실행

```
npm run storybook
```

스토리북을 실행했을 때 브라우저에 잘 뜨면 초기 셋팅은 성공이다. 


# storybook 의 기본 폴더 구조 

처음 nextjs 프로젝트에 storybook을 설치하면 아래와 같은 구조를 가지고 있다. 

```bash
├─ .storybook
│  ├─ main.ts
│  └─ preview.ts
├─ src 
│  ├─ pages
│  │  └─ ...
│  ├─ stories
│  │  ├─ assets
│  │  ├─ button.css
│  │  ├─ button.stories.ts
│  │  ├─ button.tsx
│  │  ├─ ...
│  │  └─ introduction.mdx
│  ├─ styles
│  │  ├─ ...

```

이 구조를 nextjs에 맞게 구조를 변경해서 사용하려고 한다. 


```bash
├─ .storybook
│  ├─ main.ts
│  └─ preview.ts
├─ src 
│  ├─ components
│  │  ├─ button
│  │  │  ├─ button.css
│  │  │  ├─ button.stories.ts
│  │  │  ├─ button.tsx
│  │  │  └─ ...
│  │  └─ ...
│  ├─ pages
│  │  └─ ...
│  ├─ stories
│  │  ├─ assets
│  │  ├─ ...
│  │  └─ introduction.mdx
│  ├─ styles
│  │  ├─ ...

```

# storybook의 전역 설정 

next.js 프로젝트에 storybook을 셋팅해서 사용하지만 storybook에 대한 전역 설정은 주로 `.storybook/main.ts`와 `.storybook/preview.ts`에 설정해준다. 

global.css 라던지 절대경로 설정 등등 

## storybook의 전역 스타일 설정 

storybook에서 전역으로 스타일을 설정해주려면 `.storybook/preview.ts`에 스타일 파일을 import 해줘야 한다. 

#### 전역 css 를 설정하는 방법 

```
import type { Preview } from "@storybook/react";
import '../src/styles/globals.css' // 전역으로 적용할 스타일 파일 import 


const preview: Preview = {
  ...
};

export default preview;

```

#### 전역 scss 를 설정하는 방법 

```
import type { Preview } from "@storybook/react";
import '../src/styles/global-style.scss'
import '../src/styles/themes.scss'

const preview: Preview = {
  ...
};

export default preview;

```

**scss의 변수를 전역으로 불러오게끔 설정하려면 여러가지 방법이 있는 데 ... scss 변수를 전역으로 불러오기 위한 설정은 `preview.ts`나 `main.ts`로 설정이 가능하지 않았다. 이는 다른 포스트에서 설명하려고 한다.** 

 `main.ts`의 `webpackFinal`에서 loader 관련 설정을 추가하는 등의 여러 방법으로 삽질을 하였으나 결론은 next.config.json에서 해결함 ... 


#### styled-components Provider 설정

styled-components의 Provider를 적용하려면 `page/_app.tsx`가 아닌 `.storybook/preview.ts`에 아래처럼 설정을 해줘야 한다. 

```
// .storybook/preview.ts

import { ThemeProvider } from 'styled-components'
import { GlobalStyle } from '../src/styles/global-style';
import { theme } from '../src/styles/theme';
import { withThemeFromJSXProvider } from "@storybook/addon-styling";

export const decorators = [
  withThemeFromJSXProvider({
    themes: {
      light: theme,
      dark: theme,
    },
    defaultTheme: "light",
    Provider: ThemeProvider,
    GlobalStyles: GlobalStyle,
  }),
];
```


자세히 보려면 아래 링크 참고. 

[storybook에 styled-components 적용하기](https://jjoostudy.github.io/2023-04-12/storybook%EC%97%90-styled-components-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)



## storybook의 절대경로 설정 

storybook에서의 절대경로 설정은 일단 tsconfig.json에 다른 typescript 프로젝트들에서 절대경로를 설정해주는 것 처럼 설정해준다. 

```
{
    ...
    "baseUrl": "./",
    "paths": {
      "@*": ["./src/*"],
      "@pages/*": ["src/pages/*"],
      "@styles/*": ["src/styles/*"],
      "@components/*": ["src/components/*"],
    }
  },
  "include": [
    "next-env.d.ts", 
    "**/*.ts", 
    "**/*.tsx",
    "**/*.config.js",
    "./src",
    ".eslintrc",
  ], 
  ...
}

```

그러고 ```.storybook/main.ts``` 파일에서도 절대경로에 대한 설정을 추가해준다. 

```
import path from "path";

module.exports = {
  stories: ["../src/**/*.mdx", "../src/**/*.stories.@(js|jsx|ts|tsx)"],
  addons: [
    ...
  ],
  ...
  webpackFinal: async (config) => {
    config.resolve.alias = {
      ...config.resolve.alias,
      "@src": path.resolve(__dirname, "../src"),
      "@styles": path.resolve(__dirname, "../src/styles"),
      "@components": path.resolve(__dirname, "../src/components"),
    };
    return config;
  },
};

```



## 참조 
[storybook 공식문서](https://storybook.js.org/blog/get-started-with-storybook-and-next-js/)



