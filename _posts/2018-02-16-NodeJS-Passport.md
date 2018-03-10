---
layout : default
tags : NodeJS
title : NodeJS-Passport
---

# Passport
---

로그인을 하다보면 facebook, naver 등을 이용한 로그인등이 있을 것이다.

## 설치

npm install passport --save

npm install passport-local --save

```{javascript}
app.use(passport.initialize()) //passport 초기화
app.use(passport.session()) //passport를 사용할때 session을 사용
var passport = require('passport')
var LocalStrategy = require('passport-local').Strategy
var hasher = bkfd2Password()
```

id에 해당하는 것은 반드시 ㅕsername으로, password는 반드시 password로 해야한다.

미들웨어는 미들웨어가 실행되면 여러 콜백함수를 리턴한다고 생각하면 쉽다.
