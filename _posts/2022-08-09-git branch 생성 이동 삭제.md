---
layout: post
title:  "git branch 생성,이동,삭제"
author: "JJoo"
comments: true
tags: git
---

## git 생성

```git branch``` 명령어로 브랜치를 생성할 수 있다. 

```bash
git branch 생성브랜치명 기준브런치명

//ex
git branch feature-01 develop
// develop을 기준으로 featrue-01이라는 브랜치 생성하기 
```


## git branch 삭제 

```git branch -D``` 명령어로 브랜치를 삭제할 수 있다. 

```bash
git branch -D 삭제할브랜치명
```

## git branch 이름 변경하기 


``` git branch -m``` 명령어로 브랜치 이름을 변경할 수 있다. 

```bash
git branch -m 기존브랜치명 새브랜치명

//ex
git branch -m feature-01 bugfix-01
// feature-01이란 브랜치 이름을 bugfix-01로 변경
```


## git branch 이동하기 

```git checkout``` 명령어로 원하는 브랜치로 이동할 수 있다. 

```bash
git checkout 작업할브랜치

//ex
git checkout feature-01
// feature-01 브랜치로 이동
```

