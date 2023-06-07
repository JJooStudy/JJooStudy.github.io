---
layout: post
title:  "git remote(저장소) 변경"
author: "JJoo"
comments: true
tags: git
---

## git 원격 저장소 확인하는 방법 

아래 명령어로 현재 설정되어 있는 원격 저장소의 url을 알 수 있다. 

```bash
git remote -v
```


## git 원격저장소 remote 변경 방법 

### 지정되어 있는 remote url이 있는 경우

```bash
git remote set-url origin '변경할 원격저장소의 url'
```

## git branch 이름 변경하기 

### 지정되어 있는 remote url이 없는 경우

```bash
git remote add origin '변경할 원격저장소의 url'
```

변경 후 ```git remote -v``` 명령어로 원격저장소 변경이 잘 되었는 지 확인할 수 있다. 

