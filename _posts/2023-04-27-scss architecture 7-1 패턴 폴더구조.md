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

- abstracts
  -  
- base 
  - 
components/
layout/
pages/
themes/

vendors/

