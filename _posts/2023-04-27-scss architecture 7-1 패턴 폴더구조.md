---
layout: post
title:  "scss architecture - 7-1 패턴 폴더구조"
author: "JJoo"
comments: true
tags: scss
---


## scss Architecture 

디자인 시스템을 제작하면서 scss를 다루게 되었다. styled-components만 다루다가 scss를 다루게 되면서 scss 폴더 구조를 어떻게 가져가야 할 지 고민을 했는 데 
scss 아키텍쳐 방식 중 제일 많이 사용되고 효율적인 구조가 **7-1 pattern** 이었다. 

사실 어떤 것이 제일 좋다! 라는 정답은 없기에 이게 최고다 할 수는 없지만 유명한 것에는 이유가 있을 테니 써보고 후에 나의 방식으로 풀어보는 게 좋은 것 같다 생각한다. 


## 7-1 pattern 

scss 가이드라인에서 제공하는 7-1 패턴은 아래의 구조를 가지고 있다. 

```bash
scss/
 ├─ abstracts/
 │   ├─ _variables.scss
 │   ├─ _functions.scss
 │   ├─ _mixins.scss
 │   ...
 ├─ base/
 │   ├─ _reset.scss
 │   ...
 ├─ components/
 │   ├─ _button.scss
 │   ├─ _select.scss
 │   ...
 ├─ layout/
 │   ├─ _header.scss
 │   ├─ _grid.scss
 │   ├─ _footer.scss
 │   ...
 ├─ pages/
 │   ├─ _home.scss
 │   ├─ _contact.scss
 │   ...
 ├─ themes/
 │   ├─ _theme.scss
 │   ├─ _admin.scss
 │   ...
 ├─ vendors/
 │   ├─ _bootstrap.scss
 │   ├─ _jquery.scss
 │   ...
 └─ main.scss
```

### 구조별 설명 

- abstracts
  - 다른 파일을 도와주는 역할 
  - variables.scss 변수를 담은 파일 
  - mixins 믹스인만 모아놓은 파일
  - functions function만 모아놓은 파일
- base 
  - 프로젝트에 전체적으로 사용되는 재설정, 표준 스타일 정의 등 의 역할 
  - reset.scss 
  - global.scss
  - typography.scss
- components 
  - 컴포넌트별 스타일
  - button, input, select 등   
- layout
  - 프로젝트 구조를 다루는 역할
  - header, footer, sider, nav 등 
- pages
  - 각 페이지에서만 사용되는 스타일
  - home, contact 등 
- themes
  - 모드별 스타일 관리
  - styled-components를 사용할 때는 theme.ts에서 color와 media query등을 관리했었는 데 scss에서 media query는 layout으로 들어가야 할 것 같다.
- vendors
  - 외부 라이브러리 스타일 
  - bootstrap, jquery 등 
- main.scss 
  - 위의 폴더들의 scss를 import해주는 root scss 
  - 다른 scss 파일들을 import 하는 용도로만 사용하고 줄바꿈으로 폴더별을 구분한다. 

또 아래 폴더 순서로 import 해야 한다고 한다. 

1. abstracts/
2. vendors/
3. base/
4. layout/
5. components/
6. pages/
7. themes/

 ex. 
 ```
 // main.scss 
 
 @import 'abstracts/_variables.scss'
 @import 'abstracts/_mixins.scss'
 @import 'abstracts/_functions.scss'
 
 @import 'base/_reset.scss'
 
 @import 'components/_button.scss'
 @import 'components/_select.scss'
 
 @import 'themes/_themes.scss'
 ...
 
 ```


7-1패턴이라고 해서 7가지를 모두 만들어 사용할 필요는 없고 위 폴더구조를 기반으로 프로젝트에 필요한 폴더만 만들어서 관리하면 된다. 


#### 참고 

[scss 가이드라인 - architecture](https://sass-guidelin.es/#architecture)

