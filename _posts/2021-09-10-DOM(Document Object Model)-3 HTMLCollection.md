---
layout: post
title:  "DOM(Document Object Model)-3 HTMLCollection"
author: "JJoo"
comments: true
tags: Javascript
---


> 인프런 웹브라우저 Javascript 강의를 들으면서 정리한 내용입니다. 


## HTMLCollection


HTMLCollection은 리턴 결과가 복수인 경우에 사용하게 되는 객체이다.

유사배열로 비슷한 사용방법을 가지고 있지만 배열은 아니다.

HTMLCollection의 목록은 실시간으로 변경된다. 



```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li>HTML</li>
      <li>CSS</li>
      <li id="active">JavaScript</li>
    </ul>
    <script>
      console.group('before');
      var lis = document.getElementsByTagName('li'); // lis = HTMLCollection
      for(var i = 0; i < lis.length; i++){
        console.log(lis[i]);
      }
      console.groupEnd();
      console.group('after');
      lis[1].parentNode.removeChild(lis[1]);
      for(var i = 0; i < lis.length; i++){
        console.log(lis[i]);
      }
      console.groupEnd();
    </script>
  </body>
</html>
```


결과

![HTMLCollection_console 결과](/images/img_HTMLCollection_console.png)


console.group('before'); console.groupEnd(); : 해당 코드 사이에 있는 코드들을 그룹핑해서 보여줌 


**목록을 삭제하거나 추가해 HTMLCollection의 목록이 변경이 되면 자동으로 반영이 되므로 재조회할 필요가 없다 .**





