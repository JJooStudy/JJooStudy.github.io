---
layout: post
title:  "async, defer"
author: "JJoo"
comments: true
tags: javascript
---


## 스크립트 적용 순서

> 스크립트를 어떻게 넣냐에 따라 같은 코드여도 작동이 다르게 될 수 있다. 


### head에 script를 넣는 방법 

- 파싱하다가 스크립트가 걸리면 스크립트 호출 -> 작동까지 한 후 남은 html을 파싱하기 때문에 html이 뜨기까지 너무 오래 걸린다.


{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
    <script src="main.js"></script>
  </head>
  <body></body>
</html>
{% endhighlight %}

![head에 script를 넣었을 때의 파싱 순서](/images/head_script.png "head에 script를 넣었을 때의 파싱 순서")


### body 끝에 script를 넣는 방법

- 화면은 일찍 뜨지만 자바스크립트 동작이 오래걸려 화면 이벤트가 작동하지 않을 수 있다.


{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
  </head>
  <body>
    <div></div>
    <script src="main.js"></script>
  </body>
</html>
{% endhighlight %}

![body 끝에 script를 넣었을 때의 파싱 순서](/images/body_script.png "body 끝에 script를 넣었을 때의 파싱 순서")


### head에 넣고 script에 async를 넣는 방법

- async는 boolean타입으로 넣기만 하면 됌
- Html 과 자바스크립트를 병렬 파싱함
- 자바스크립트를 불러오고 바로 실행해서 그사이에 공백이 생길 수 있다


{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
    <script async src="main.js"></script>
  </head>
  <body>
    <div></div>
  </body>
</html>
{% endhighlight %}

![head에 넣고 script에 async를 넣었을 때의 파싱 순서](/images/head_async_script.png "head에 넣고 script에 async를 넣었을 때의 파싱 순서")


### head에 넣고 script에 defer를 넣는 방법

- html과 병렬 파싱을 하는 데 자바스크립트를 불러오고 html까지 파싱 되고나서 자바스크립트를 작동시킨다.


{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
    <script defer src="main.js"></script>
  </head>
  <body>
    <div></div>
  </body>
</html>
{% endhighlight %}

![head에 넣고 script에 defer를 넣었을 때의 파싱 순서](/images/head_defer_script.png "head에 넣고 script에 defer를 넣었을 때의 파싱 순서")


### head의 여러 스크립트를 async로 불러올 경우 

- 모든 스크립트를 병렬로 불러온다. 각 스크립트의 용량별로 호출되는 시기가 달라서 빨리 호출되는 순서대로 빨리 작동을 시켜버린다.
- 작동의 우선 순위가 있는 경우 문제가 생길 수 있다. 


{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
    <script async src="a.js"></script>
    <script async src="b.js"></script>
    <script async src="c.js"></script>
  </head>
  <body>
    <div></div>
  </body>
</html>
{% endhighlight %}

![head의 여러 스크립트를 async로 불러올 경우의 파싱 순서](/images/head_async_multi_script.png "head의 여러 스크립트를 async로 불러올 경우의 파싱 순서")


### head의 여러 스크립트를 defer로 불러올 경우 

- html을 파싱하면서 모든 스크립트를 불러온다.
- html이 모두 파싱되고 실행될 때 이미 불러온 스크립트를 적용한 순서대로 작동시킨다.

{% highlight javascript %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <title>Document</title>
    <script defer src="a.js"></script>
    <script defer src="b.js"></script>
    <script defer src="c.js"></script>
  </head>
  <body>
    <div></div>
  </body>
</html>
{% endhighlight %}

![head의 여러 스크립트를 defer로 불러올 경우의 파싱 순서](/images/head_defer_multi_script.png "head의 여러 스크립트를 defer로 불러올 경우의 파싱 순서")


