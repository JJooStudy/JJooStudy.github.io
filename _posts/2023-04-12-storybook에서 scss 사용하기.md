---
layout: post
title:  "storybook에서 scss 사용하기"
author: "JJoo"
comments: true
tags: storybook next.js scss
---


# storybook에 scss 적용하는 방법 

nextjs + storybook 환경에서 scss를 설정해서 사용하려면 4가지 패키지 설치가 필요하다. 

- style-loader 
- css-loader 
- sass-loader 
- @storybook/preset-scss

위 3가지는 webpack에서 필요한 loader, 아래 1가지는 storybook에서 필요한 addon 패키지다. 

아래 명령어로 설치해주자. 

```
npm install @storybook/preset-scss css-loader sass sass-loader style-loader --save-dev
```

`.storybook/main.tx`에서 addons 설정에 @storybook/preset-scss를 추가해준다. 

```
//.storybook/main.tx 
module.exports = {
  ...
  addons: [
    "@storybook/addon-links",
    "@storybook/addon-essentials",
    "@storybook/addon-interactions",
    '@storybook/addon-styling',
    // '@storybook/preset-scss',
  ],
  ...
};
```

* 이것저것 테스트 하면서 알게 된 것은 storybook 7버전에서는 @storybook/preset-scss addon을 추가하지 않아도 scss가 작동하는 것에 문제가 없다는 것이다. 

* 지금은 @storybook/preset-scss를 제거하고 사용하는 중이다. 진행하다가 문제 발생시 내용 추가할 예정 


## next.js + storybook 환경에서 scss 변수 전역으로 사용하는 방법 

scss를 사용 중에 `_variables.scss`파일에서 선언한 변수를 사용하려는 데 사용하는 파일에서 일일이 `_variables`파일을 import 해와야 했는 데... 

파일 내부에서 매번 import 해오는 게 번거롭기도 하고 @import는 import 할 때마다 css가 방출되어 컴파일되는 양이 많아져 컴파일 시간이 길어지는 단점이 있다고 한다. 

그래서 scss에서 @import를 권장하지 않는 다고 하여 변수를 전역으로 사용할 수 있는 방법을 고민했다. 

next.config.json에서 전역으로 사용할 scss 파일을 설정해줄 수 있다. 

```
// next.config.json
const path = require('path');

const nextConfig = {
  reactStrictMode: true,
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')], 
    prependData: `@import "./src/styles/scss/_variables.scss";`, // 전역으로 사용할 변수, 함수, 믹스인 등을 넣어줄 수 있다. 
  },
};

module.exports = nextConfig;
```

* 참고로 @import 대신 @use를 권장하고 있고 이에 대한 내용은 추후에 게시글을 추가할 예정 




