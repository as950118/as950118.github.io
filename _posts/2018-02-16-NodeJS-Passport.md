---
layout:default
tags : NodeJS
title : NodeJS-Passport
---

#Passport
---

로그인을 하다보면 facebook, naver 등을 이용한 로그인등이 있을 것이다.

##설치

npm install passport-local --save

```{javascript}
app.use(passport.initialize()) //passport 초기화
app.use(passport.session()) //passport를 사용할때 session을 사용
var passport = require('passport')
var LocalStrategy = require('passport-local').Strategy
var hasher = bkfd2Password()
```