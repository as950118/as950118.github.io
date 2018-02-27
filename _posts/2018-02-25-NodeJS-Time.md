---
layout : default
tags : NodeJS
title : NodeJS-Time
---

#Nodejs - Time <hr>

date-utils 라는 모듈을 사용한다.

##설치

npm install date-t=ituls --save

##코드

require('date-utils')
var newDate = new Date()
var date = newDate.toFormate('YYYY-MM-DD HH24:MM:SS')