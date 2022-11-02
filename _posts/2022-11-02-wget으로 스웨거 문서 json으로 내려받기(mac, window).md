---
layout: post
title:  "wget으로 스웨거 문서 json으로 내려받기"
author: "JJoo"
comments: true
tags: wget
---


redoc으로 스웨거 문서 등을 변환하는 작업을 하면서 스웨거의 api 문서를 json으로 받아야 했다. 그러면서 알게된 wget.

## wget이란?

Wget은 Web get의 약자로 리눅스 명령어이다. 명칭 그대로 HTTP 또는 HTTPS, FTP 통신을 사용해서 web상의 파일 또는 컨텐츠를 다운로드 해준다.

wget을 사용하면 여러 파일을 한번에 다운로드 하거나 웹 페이지들을 순회하며 여러 컨텐츠를 자동으로 다운받을 수 있다. 찾아보니 크롤링에 많이 쓰이는 듯 하다. 


## mac에서 wget 사용하기 

mac에서는 brew를 사용해 간단하게 설치할 수 있다. 

``` $ brew install wget ```

brew가 없다면 설치하여 사용하자. 

## window에서 wget 사용하기 

window에서 wget 사용하는 게 제일 귀찮다 ... 

회사에서 mac과 window를 둘다 사용하고 있어서 mac에서 wget을 사용했는 데 나중을 위해 window에서의 사용법도 기록해본다. 

[https://eternallybored.org/misc/wget/](https://eternallybored.org/misc/wget/) 에서 자신이 사용하는 운영체제에 맞는 윈도우용 wget를 다운받는다. 

다운받은 wget.exe 파일을 C:\Windows\System32에 넣어준다. 

system path가 기본적으로 지정된 시스템폴더에 넣어줘야 cmd에서 아무 경로에서나 wget을 사용할 수 있다. 

cmd를 켜서 wget을 쳐보면 잘 적용이 되었는 지 확인할 수 있다. 


## wget 사용법 

wget 사용은 간단하다.

기본형식

``` wget 다운받을url ```

다운받을 파일명과 위치 지정 및 기존파일에 덮어쓰기 옵션 

``` wget -O ~/경로/파일명.json 다운받을url ```




