---
layout: post
title:  "git branch 생성(local, remote)"
author: "JJoo"
comments: true
tags: git
---

## git branch 생성하기 (local repository)

`git branch 브랜치명`를 이용해 브랜치 생성을 할 수 있다. 

ex. feature-01 라는 이름 브랜치 생성 후 해당 브랜치로 이동(checkout)

```bash
git branch feature-01
git checkout feature-01 
```

`git branch -b`를 이용해 브랜치 생성과 이동을 동시에 할 수 있다. 

```bash
git branch -b feature-01
```

## 원격 저장소(remote repository)에 branch 생성하기 

위의 과정은 local에만 브랜치를 생성하는 것이다. 

local에서 브랜치를 생성 후 원격 저장소에도 생성 및 연결을 해줘야 한다. 

원격저장소(remote repostiory)가 지정이 되어 있다면 `git push origin 브랜치명`으로 push해주면 원격저장소(remote repostiory)에도 branch가 생성된다. 

```bash
git push origin feature-01
```

원격저장소(remote repostiory)가 지정이 되어 있지 않다면 먼저 `remote add`를 사용해 원격저장소를 지정해주고 push를 해야 한다.

```bash
git remote add origin http://원격저장소url
git push origin feature-01
```

이렇게 하면 local과 remote에 각각 같은 이름의 브랜치가 생성된 상태가 된다.

이 local branch와 remote branch를 연동하는 작업이 또 필요하다.

```bash
git branch --set-upstream-to origin/feature-01
```

