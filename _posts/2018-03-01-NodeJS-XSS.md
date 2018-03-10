---
layout : default
tags : NodeJS
title : "NodeJS-XSS"
---

#Nodejs-XSS

##XSS란?

Cross-site scripting의 줄임말이다.

web app에서 많이 발생되는 문제이다.

쉽게 설명하자면 게시판같이 어떠한 글을 입력할 수 있는 곳에 `<script>`같은 명령문을 삽입하여 권한을 탈취하거나 특정 정보에 접근하는 등의 해킹 기법이다.

NodeJS에서는 XSS 모듈을 이용해 쉽게 방지할 수 있다.

##설치

npm install xss --save


##코드

###Javascript
```{javascript}
var xss = require('xss')
var html = xss('<script>alert('XSS')</script>')
console.log(html)
```

###html
```{html}
<script src="https://cdnjs.cloudflare.com/ajax/libs/js-xss/0.3.3/xss.min.js"></script>
<script>
var html = filterXSS('<script>alert("xss");</scr' + 'ipt>');
alert(html);
</script>
```

각각이 어떻게 실행되는지 확인해보자.

또 xss 모듈을 사용하지 않고 `<script>...`를 하였을 경우 어떻게 실행되는지를 비교해보자.

##결론

NodeJS의 모듈을 참 편리하다