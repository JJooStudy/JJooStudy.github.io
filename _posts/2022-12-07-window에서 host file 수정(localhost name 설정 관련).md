---
layout: post
title:  "window에서 host file 수정(localhost 설정 관련)"
author: "JJoo"
comments: true
tags: window
---

# window에서 host file 수정해보자 

localhost로 개발을 하다 도메인으로 띄워야 하는 상황이 생겨서 설정을 해보았다. 

로컬 환경에서 개발을 바로바로 테스트를 할 수 있으면서 도메인으로 띄워야 했는 데 

맥에서는 [가스 마스크](https://www.macupdate.com/app/mac/29949/gas-mask) 를 사용해서 간단하게 설정할 수 있지만 

윈도우 환경에서는 host file에 설정을 추가해줘야 한다. 

방법은 간단하다.

### 메모장을 우클릭해서 관리자 모드로 열어준다. 

![img_window_host_file_setting_01](/images/img_window_host_file_setting_01.png 'img_window_host_file_setting_01')

### 파일열기로 아래 링크로 들어가서 host 파일을 열어준다. 

![img_window_host_file_setting_02](/images/img_window_host_file_setting_02.png 'img_window_host_file_setting_02')

```C:\Windows\System32\drivers\etc```

etc 파일에 들어가면 아무것도 보이지 않지만 오른쪽 하단에 모든 파일로 선택해주면 여러 설정  파일들이 보여진다. 

그 중 host 파일을 열여준다. 


### 설정하고 싶은 url을 적어주고 저장한다. 

```
127.0.0.1 	설정하고싶은url

// ex
127.0.0.1 	local-jjoostudy.blabla.com
```


### localhost 대신 설정한 url을 브라우저에 입력하면 프로젝트가 보인다. 

프로젝트 실행 후 localhost:8080 대신 local-jjoostudy.blabla.com:8080을 입력하면 실행한 프로젝트가 보인다. 

코드 수정시 바로바로 반영되는 것도 확인할 수 있다. 









