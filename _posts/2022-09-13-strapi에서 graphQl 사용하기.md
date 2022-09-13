---
layout: post
title:  "strapi에서 graphQl 사용하기"
author: "JJoo"
comments: true
tags: strapi
---

## strapi에서 graphQl plugin 설치하기. 

strapi는 graphQl도 지원한다. 

strapi admin의 marketplace에서 graphQl을 찾을 수 있다. 

![strapi marketplace graphql](/images/img_strapi_marketplace_graphql.png "strapi marketplace graphql")

아래 명령어로 설치 할 수 있다. 

```
npm install @strapi/plugin-graphql
```

[관련 plugin link](https://market.strapi.io/plugins/@strapi-plugin-graphql).

> 여기서 한가지 문제가 있다. 
> strapi 작업을 진행하던 중에 graphQl을 설치하게 되면 npm 모듈 중 어느 부분과 충돌(?)하여 설치가 중단된다. 
> 이럴 때는 node_modules와 package-lock.json을 삭제, npm i로 재 설치 후 다시 graphQl을 설치해주면 잘 설치가 된다. 
> (여러번 테스트 해봄 ..... )


## graphQl 실행하기(사용하기)

graphQl이 정상적으로 설치가 되었다면 아래 링크로 접속이 된다. 

[http://localhost:1337/graphql](http://localhost:1337/graphql)

![strapi graphql main](/images/img_strapi_graphql_main.png "strapi graphql main")





