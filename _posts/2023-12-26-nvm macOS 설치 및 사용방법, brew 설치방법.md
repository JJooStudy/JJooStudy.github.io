---
layout: post
title:  "nvm 윈도우 설치 및 사용방법"
author: "JJoo"
comments: true
tags: nvm
---


# macOS에서 nvm 설치하기 전에 brew 설치하기

회사에서 간간히 맥북을 쓰다가 이번에 맥북을 구매했다. 

때문에 macOS 관련 게시글을 간간히 작성할 듯 하다. 

mac에서 nvm을 설치를 할때는 Homebrew라는 패키지 관리자를 사용한다. brew를 사용해서 여러 프로그램들을 간편하게 설치 등 관리할 수 있다. 

Homebrew는 아래 명령어로 설치할 수 있다. 

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

위 명령어를 치면 설치가 진행되는 데 맥의 비밀번호를 요구한다.

설치를 완료하고 나면 이어서 brew를 환경 변수로 등록을 해줘야 brew를 제대로 사용할 수 있다. 

brew 환경변수 등록 명령어 
```
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
```

등록한 환경변수 반영 명령어 
```
source ~/.zshrc
```

여기까지 하고 brew version을 확인해 제대로 설치됐는 지 확인할 수 있다. 

```
$ brew --version
Homebrew 4.0.16
```

이렇게 버전이 확인되면 제대로 설치가 완료된 것이다.


# nvm 설치 방법 

brew를 이용해 nvm을 설치한다. 

```
brew install nvm
```

설치 후 nvm도 환경변수 설정이 필요하다. 




# nvm 사용방법 

터미널에 `nvm` 명령어를 쳐보면 nvm 사용법에 대한 내용이 나온다. 

`nvm list`를 입력하면 nvm으로 관리할 수 있는 node 버전 리스트가 나오는 데 node를 설치한 적이 없으면 아무것도 나오지 않는 다. 


# nvm 으로 node 설치하기 

```
nvm install [원하는버전]
```


# nvm 으로 node 버전 설정하기 

```
nvm use [사용하려는버전]
```


# nvm으로 특정 node 버전 삭제하기 

```
nvm uninstall [삭제하려는버전]
```


[Homebrew homepage](https://brew.sh/ko/)
