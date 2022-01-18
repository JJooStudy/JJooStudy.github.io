---
layout: post
title:  "pakage.json 최신버전 업데이트"
author: "JJoo"
comments: true
tags: react
---

## pakage.json 업데이트?

일반적으로 모듈 업데이트를 하기 위해서는 

```
npm update '패키지명'
```

으로 업데이트가 필요한 모듈을 하나씩 업데이트한다. 

보통 의존성을 한번에 일괄적으로 업데이트를 하는 것은 모듈끼리의 호환성에 대한 이슈가 생길 수 있기 때문에 `npm`에서 공식적으로 지원을 하고 있지 않다.

하지만 개발하면서 일괄적으로 업데이트를 해야할 때가 있다.

그럴 때 사용할 수 있는 `npm-check-updates` 라이브러리가 있다. 

사용법은 아주 간단하다.

설치
  ```
  npm install npm-check-updates
  ```
  
패키지 설치 후 터미널에 `ncu` 를 쳐보면 업데이트를 해야할 모듈이 무엇인지 목록으로 보여준다. 

`ncu -u`를 쳐야 업데이트가 되야할 모듈들이 업데이트 버전으로 `pakage.json`에 기록된다. 

여기까지는 `pakage.json`에 기록만 된 것이고 `node-modules`에 반영된 것은 아니므로 `npm install`을 실행 해줘야 한다.

[npm-check-updates 공식문서](https://www.npmjs.com/package/npm-check-updates)
