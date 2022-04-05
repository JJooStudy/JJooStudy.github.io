---
layout: post
title:  "git commit 취소"
author: "JJoo"
comments: true
tags: git
---

## git commit 취소하기 

올린 commit을 취소하고 싶을 때가 있다. 

어떤 파일을 빼먹었을 때나, commit을 하고나서 추가로 수정을 했거나 등등 

이럴때는 `git reset HEAD^`를 사용해서 commit을 취소할 수 있다. 

git commit을 취소하는 방법에는 몇가지가 있다. 

#### commit을 취소하고 해당 파일들을 staged 상태로 돌리기 

```bash
git reset --soft HEAD^
```

`git reset --soft HEAD^`시 `More?` 하고 묻는 경우가 있다. 

`git reset --soft HEAD~1`로 갯수를 명시해주면 묻지 않고 작동한다. 

`git reset --soft HEAD^^` 이렇게 ^을 두번 써줘도 된다고 한다. (사용해보지는 않았다.)



#### commit을 취소하고 해당 파일들을 unstaged 상태로 돌리기 

```bash
git reset --mixed HEAD^
//or
git reset HEAD^
```

#### 마지막 commit 항목 2가지를 한번에 취소하고 해당 파일들을 unstaged 상태로 돌리기 

```bash
git reset HEAD~2 
```

#### commit을 취소하고 해당 파일들을 unstaged 상태로 디렉토리에서도 삭제하기 

```bash
git reset --hard HEAD^
```

`--hard`는 작업물을 모두 삭제하는, 작업의 이전 상태로 돌리게 되니 주의해서 사용해야 한다. 


