---
layout: post
title:  "strapi api 사용하기"
author: "JJoo"
comments: true
tags: strapi
---

## strapi api 사용하기 

백엔드는 strapi로 프론트엔드는 next.js + react-query 로 구성했다. 

strapi로 만든 api를 프론트에서 사용할 때 strapi에서 documentation plugin을 추가해서 사용하면 스웨거를 사용할 수 있고, 스웨거를 사용하면 편하게 api를 사용할 수 있다. 

[documentation plugin을 추가/설정 하기](https://jjoostudy.github.io/2022-08-30/strapi-%EC%8A%A4%EC%9B%A8%EA%B1%B0-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0)

## api 조회시 엮여 있는 하위 데이터들도 같이 호출하는 방법 

strapi api type 생성 시 type간 관계들도 설정을 해줄 수 가 있다. ( [생성한 api type별 관계 지정하기](https://jjoostudy.github.io/2022-08-30/strapi-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0#%EC%83%9D%EC%84%B1%ED%95%9C-api-type%EB%B3%84-%EA%B4%80%EA%B3%84-%EC%A7%80%EC%A0%95%ED%95%98%EA%B8%B0) )

위처럼 관계가 설정이 되어 있다면 api를 조회할 때 해당 data에 속해 있는 하위 data들도 같이 조회할 수 있다. 

![swegger get api ex](/images/img_strapi_get_api_exam.png "swegger get api ex")

스웨거의 예시에서 내가 설정한 data의 모든 관계 data들을 보여주는 데 실제로 api를 호출하면 호출한 data만 나오고 그 해당 data에 관계가 연결되어 있는 하위 data는 조회되지 않는다. 

하위 data도 같이 조회하려면 몇가지 parameter를 덧붙여서 조회해야 한다. 

```
A 
├ B 
│ └ B-A
└ C
```
위 구조의 data를 조회한다고 가정해보자 


### 모든 하위 1레벨 data 조회하기 

```
localhost:1337/api/A?populate=*
```

`?populate=*`를 붙여서 조회하면 A에 엮여있는 B와 C까지 조회된다. B-A는 조회되지 않는다. 

### 하위 2레벨 data까지 조회하기 

프론트에서 화면을 구성하다보니 B-A까지 한번에 조회해서 화면을 그리고 싶어졌다. 

```
localhost:1337/api/A?populate[B][populate]=*
```

위 코드처럼 A를 조회할 때 좀더 깊게 조회하고 싶은 요소를 대괄호에 넣어 조회하면 A를 조회할 때 B와 B-A까지 한번에 조회할 수 있다. 


[참고 공식 문서](https://docs.strapi.io/developer-docs/latest/developer-resources/database-apis-reference/rest/populating-fields.html#component-dynamic-zones)

