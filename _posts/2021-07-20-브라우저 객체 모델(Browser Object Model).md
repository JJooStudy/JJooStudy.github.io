---
layout: post
title:  "BOM(Browser Object Model)"
author: "JJoo"
comments: true
tags: Javascript
---



## BOM(Browser Object Model)


BOM(Browser Object Model)이란 ?

웹브라우저의 창이나 프래임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단이다. 

BOM은 전역객체인 Window의 프로퍼티와 메소드들을 통해서 제어할 수 있다. 

객체 모델 종류에는  `window`(최상위), `location`, `navigator`, `history`, `screen`, `document`

DOM(Document Object Model) 으로 통합해서 칭하기도 함

![Object Model](/images/img_browser_object_model.png "Object Model")



## 전역객체 window 

window객체는 브라우저에서 최상위 객체이다.

Window 객체는 식별자 `window`를 통해서 얻을 수 있다. 또한 생략 가능하다. 

Window 객체의 메소드인 `alert`을 호출하는 방법과 전역변수 `a`에 접근하는 방법은 아래와 같다

```html
<!DOCTYPE html>
<html>
  <script>
    alert('Hello world');
    window.alert('Hello world');
  </script>
  <body>
  </body>
</html>

<!DOCTYPE html>
<html>
  <script>
    var a = 1;
    alert(a);
    alert(window.a);
  </script>
  <body>
  </body>
</html>
```


객체를 만든다는 것은 결국 window 객체의 프로퍼티를 만드는 것과 같다.


```
<!DOCTYPE html>
<html>
  <script>
    var a = {id:1};
    alert(a.id);
    alert(window.a.id);
  </script>
  <body>
  </body>
</html>
```

전역변수와 함수가 사실은 window 객체의 프로퍼티와 메소드라는 것이다. 

또한 모든 객체는 사실 window의 자식이라는 것도 알 수 있다. 

이러한 특성을 ECMAScript에서는 Global 객체라고 부른다. ECMAScript의 Global 객체는 호스트 환경에 따라서 이름이 다르고 하는 역할이 조금씩 다르다. 

웹브라우저 자바스크립트에서 Window 객체는 ECMAScript의 전역객체이면서 동시에 웹브라우저의 창이나 프레임을 제어하는 역할을 한다.




## 사용자와 커뮤니케이션 하기

HTML은 `form`을 통해서 사용자와 커뮤니케이션할 수 있는 기능을 제공한다. 자바스크립트에는 사용자와 정보를 주고 받을 수 있는 간편한 수단을 제공한다


#### alert 

경고창이라고 부른다. 사용자에게 정보를 제공하거나 디버깅의 용도로 많이 사용한다.

경고창이 실행되어 있는 동안은 다음 스크립트가 실행되지 않는다.

![alert](/images/img_window_alert.png "alert")


#### confirm

확인이라고 하는 컴펌창

경고창이랑은 다르게 확인과 취소 버튼이 있다. 

![confirm](/images/img_window_confirm.png "confirm")

확인버튼을 누르면 `true`, 취소를 누르면 `false`를 반환한다. 

사용자의 응답에 따른 분기처리를 할 수 있다.  


```
<!DOCTYPE html>
<html>
  <body>
    <input type="button" value="confirm" onclick="func_confirm()" />
    <script>
      function func_confirm(){
        if(confirm('ok?')){
          alert('ok');
        } else {
          alert('cancel');
        }
      }
    </script>
  </body>
</html>
```



#### prompt 

사용자가 입력한 값을 받아서 자바스크립트가 얻을 수 있는 기능 

![prompt](/images/img_window_prompt.png "prompt")

```javascript
prompt('id?');
```

텍스트 필드에 입력한 값을 반환하여 보여준다. 

```
<!DOCTYPE html>
<html>
  <body>
    <input type="button" value="prompt" onclick="func_prompt()" />
    <script>
      function func_prompt(){
        if(prompt('id?') === 'egoing'){
          alert('welcome');
        } else {
          alert('fail');
        }
      }
    </script>
  </body>
</html>
```

