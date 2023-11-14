---
layout: post
title:  "javascript로 영역의 width, height 가져오는 방법"
author: "JJoo"
comments: true
tags: Javascript
---


# javascript로 width나 height style을 읽어올 영역을 지정한다. 

```
// id 값으로 영역 지정하기
const idBox = document.getElementById(id);

// class 값으로 영역 지정하기
const classBox = document.querySelector(class)
```

javascript로 영역을 지정하는 방법은 여러가지가 있지만 class name이나 id 값으로 원하는 영역을 지정해줄 수 있다. 

하지만 class name의 경우에는 겹치는 class name이 있을 수 있으니 id값으로 지정하는 게 원하는 영역을 제대로 지정해줄 수 있다. 


# width style 값을 가져오는 방법 

```
// clientWidth : padding을 포함하는 영역의 너비값을 가져옴 
const idBoxWidth01 = idBox.clientWidth;

// offsetWidth : padding뿐 아니라 border, scrollbar도 포함한 영역의 너비값을 가져온다. 
const idBoxWidth02 = idBox.offsetWidth;

```


# height style 값을 가져오는 방법 

```
// clientHeight : padding을 포함하는 영역의 높이값을 가져옴 
const idBoxHeight01 = idBox.clientHeight;

// offsetHeight : padding뿐 아니라 border, scrollbar도 포함한 영역의 높이값을 가져온다. 
const idBoxHeight02 = idBox.offsetHeight;

// scrollHeight : overflow로 화면에 표시되지 않은 컨텐츠의 높이값도 가져옴. padding값을 포함한다.  
const idBoxHeight03 = idBox.scrollHeight;
```


# getBoundingClientRect()를 사용하여 width와 height 동시에 가져오는 방법

```
// idBox의 너비, 높이, 왼쪽
const idBoxSize = idBox.getBoundingClientRect();

const idBoxWidth = idBoxSize.width;

const idBoxHeight = idBoxSize.height;

```
