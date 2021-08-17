---
layout: post
title:  "beforeunload event"
author: "JJoo"
comments: true
tags: Javascript
---


## beforeunload event

> 사용자가 현재페이지를 벗어나거나 새로고침을 감지하는 이벤트 

사용자가 브라우저 탭을 닫거나, 새로고침 할 시 사용자에게 브라우저를 떠날 것인지 확인 요청을 합니다.

- 새로고침
- 뒤로가기
- 브라우저 닫기
- 페이지 이동 
- 등등

기본 사용 방법

```javascript
window.addEventListener('beforeunload', (event) => {
  // 표준에 따라 기본 동작 방지
  event.preventDefault();
  // Chrome에서는 returnValue 설정이 필요함
  event.returnValue = '';
});

// 혹은 

window.onbeforeunload = function() {
  return false;
};

// 현재 크롬에서는 return이 적용되지 않음.
window.onbeforeunload = function() {
  return "저장되지 않은 변경사항이 있습니다. 정말 페이지를 떠나실 건 가요?";
};


```

과거에는 return에 문자열을 담으면 그 문자열을 반환해 브라우저에서 보여줬으나 현재에서는 문자열이 적용되지 않아 문구를 커스터마이징 할 수 없습니다.


또 react에서 사용시 브라우저 닫기와 컴포넌트 안에서의 페이지 이동은 감지해내지만 다른 컴포넌트를 사용한 페이지 이동과 뒤로가기 등은 감지하지 못합니다.

react의 원리가 virtual DOM을 이용하기 때문이라고 추측됩니다. 


## react와 virtual DOM에 대해 이해하기 좋은 영상

[react와 virtual DOM](https://www.youtube.com/watch?v=BYbgopx44vo)


[react에서의 beforeunload event 보러가기](https://jjoostudy.github.io/2021-08-17/react%EC%97%90%EC%84%9C%EC%9D%98-beforeunload-event)
