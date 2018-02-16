---
layout:default
tags:NodeJS
title:NodeJS-Password
---

#암호화
---

sha256를 이용해 암호화에 대한 이해를 해보자

##설치

npm install sha256 --save

##실습

```{javasciprt}
var sha = require('sha256')
sha256('javasciprt')
```

##Salt

salt라는 건 암호를 조금 더 복잡하게 하는 것이다.

```{javascript}
var salt = 'asdfg!12345' //임의의 값
sha256('javascript' + salt)
```

한층 더 복잡한 값이 생성될 것이다.

#양방향 암호화
---

pbkdf2-password 모듈을 이용한 암호화를 알아보자

##설치

npm install pbkdf2-password --save

##실습

```{javascript}
var bkfd2Password = require('pbkdf2-password')
var hasher = bkfd2Password()
hasher({password:'1111'}, function(err, pass, salt, hash){
	console.log(err, pass, salt, hash)
})
```

순서대로 에러 여부, 암호, 자동으로 생성한 salt 값, 단방향으로 암호화한 값이 출력될 것이다.

그리고 실행시마다 salt 값이 달라지고 그에 따라 암호화한 값도 달라진다.

