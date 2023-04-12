---
layout: post
title:  "storybook에 styled-components 적용하기"
author: "JJoo"
comments: true
tags: storybook next.js styled-components
---


현 게시글은 아래의 styled-components의 설치 및 필수 파일이 추가되어 있는 환경에서 진행된 내용이다. 

styled-component 셋팅하는 방법 보러가기 -> [styled-component 셋팅하는 방법](https://jjoostudy.github.io/2021-10-29/Next.js%EC%97%90-styled-components-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)


# storybook에 styled-components 적용하는 방법 

storybook은 전체적인 설정 적용을 하려면 `storybook/main.ts`와 `.storybook/preview.ts`에서 설정들을 셋팅해줘야 한다. 

styled-components를 storybook에서 사용하려면 위 두 파일에서 styled-components에 대한 설정들을 추가 해줘야 하는 데 구글 검색시 많이 나오는 
아래와 같이 `preview.js` 에서 `decorators`에 `<ThemeProvider>`를 컴포넌트 형식으로 적용하는 방법 등 은 `ThemeProvider` 설정이 제대로 되지 않았다. 

```
decorators: [
    (Story) => (
      <ThemeProvider theme={theme}>
        <Story />
      </ThemeProvider>
    ),
  ],
```

storybook의 공식문서에 나와있는 styled-components의 theme 설정하는 방법을 찾아보니 `addon-styling` 라이브러리로 설정을 하는 방법을 찾았다. 


## addon-styling 설치 

```
npm i -D @storybook/addon-styling
```


## `storybook/main.ts`와 `.storybook/preview.ts` 에 설정 

설치 후 `storybook/main.ts` 에 `'@storybook/addon-styling'`을 추가한다. 

```
// storybook/main.ts

module.exports = {
  stories: ['../stories/**/*.stories.mdx', '../stories/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [ ... , '@storybook/addon-styling'],
};
```

`.storybook/preview.ts`에 `withThemeFromJSXProvider`를 이용해서 `decorators` 설정을 추가한다. 

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

아직 라이트/다크 모드에 대한 설정을 추가 하지 않았기 때문에 모두 `theme`로 일단 설정해 기본 설정을 마쳤다. 


# global-style.ts에서 theme에 대한 DefaultTheme 오류 

ThemeProvider까지 잘 적용하고 나도 global-style.ts에서 `const { colors } = theme;`으로 불러오는 `colors`에 아래와 같은 에러 메세지가 뜬다. 

```
Property 'colors' does not exist on type 'DefaultTheme'.
```

이 에러는 `theme.ts`에서 `import { DefaultTheme } from 'styled-components';` 에서 경로를 `import { DefaultTheme } from 'styled-components/native';`로 변경해주면 해결된다. 




## 참고 

[storybook 공식문서 - Integrate Styled Components and Storybook](https://storybook.js.org/recipes/styled-components#recipe-section)

[storybookjs/addon-styling github](https://github.com/storybookjs/addon-styling/blob/main/docs/api.md#withthemefromjsxprovider)


