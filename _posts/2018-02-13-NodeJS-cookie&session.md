---
layout : default
tags : NodeJS
title : cookie
---

# Cookie

web browser이 response을 하면서 모든 data를 user의 저장공간에 저장하는 방식

## 설치

npm install cookie-parser --save

## Cookie의 보안
cookieParser('임의의 값') 으로 암호화를 함

cookie를 전송받는 부분은 req.cookies. 도 req.signedCookies. 로 수정하여 해독함

cookie를 생성하는 부분은 res.cookies( , {signed:true}) 로 수정

아무리 암호화하고 https로 배달사고를 방지해도 사고의 방지를 장담할 수 없으므로 중요한 정보는 cookie가 아닌 session을 이용함

**user와 server의 저장공간을 적절히 사용하는 session을 이용하는게 더 안전함**

###cookie_counter.js
```{javascript}
var express = require('express')
var cookieParser = require('cookie-parser')
var app = express()
app.listen(3003, function(){
  console.log("3003 port")
})
app.use(cookieParser())
//카운트
app.get('/count', function(req,res){
  if(req.cookies.count){
    var count = parseInt(req.cookies.count)
  }
  else{
    var count = 0;
  }
  count = count + 1
  res.cookie('count', count)
  res.send('count : ' + count)
})
//
//쇼핑카트
var products = {
  1:{title:'History of web 1'},
  2:{title:'Next web'}
}
app.get('/products', function(req, res){
  var output = ''
  for(var name in products){
    output += `
      <li>
        <a href='/cart/${name}'>${products[name].title}</a>
      </li>
    `
  }
  res.send(`
    <h1>Products</h1>
    <ul>${output}</ul>
    <a href='/cart'>Cart</a>
    `)
})
app.get('/cart/:id', function(req, res){
  var id = req.params.id
  if(req.cookies.cart){
    var cart = req.cookies.cart //사용자의 컴퓨터에 담을 정보
  }
  else{
    var cart = {} //최초로 실행될대는 비어있는 변수
  }
  if(!cart[id]){ //아무것도 없는 값일때는 0으로 세팅해줌
    cart[id] = 0
  }
  cart[id] = parseInt(cart[id]) + 1 //쇼핑카트에 담으면 1 증가 , parseInt는 문자를 숫자로 강제변환하는 JS 문법
  res.cookie('cart', cart)
  res.redirect('/cart')
})
app.get('/cart', function(req, res){
  var cart = req.cookies.cart
  if(!cart){
    res.send('empty')
  }
  else{
    var output = ''
    for(var id in cart){
      output +=`
        <li>${products[id].title} (${cart[id]})</li>
      `
    }
    res.send(`
      <h1>Cart</h1>
      <ul>${output}</ul>
      <a href='/products'>Products list</a>
      `
    )
  }
})
//

```

# Session

user의 식별자값만을 user의 저장공간에 저장

각각의 식별자에 대응하는 실제 data는 serv에 저장

필요할 때마다 server에서 가져오는 방식

까서봐보자

connect.sid처럼 나타날 것이다

둘다 cookie를 이용하긴 하지만

session은 직접 data를 저장하는 것이 아니라 id만을 저장한다는 것을 알수 있다.

##설치

npm install express-session --save

###session_counter.js
```{javascript}
var express = require('express')
var session = require('express-session')
var bodyParser = require('body-parser')
var app = express()
app.listen(3003, function(){
  console.log("3003 port")
})
app.use(bodyParser.urlencoded({extended:false}))
app.use(session({
  secret: 'heonjinjeong', //암호화를 위한 임의의값
  resave: false, //session id를 시작때마다 새롭게 발급할지
  saveUninitialized: true //session id를 실제로 사용하기 전에 발급할지말지(un)
}))
//카운트
app.get('/count', function(req,res){
  if(req.session.count){
    req.session.count++
  }
  else{
    req.session.count=1
  }
  res.send('count : '+ req.session.count)
})
//
//로그인
app.get('/auth/login', function(req, res){
  var output = `
    <h1>Login</h1>
    <form action='/auth/login' method='post'>
      <p>
        <input type='text' name ='username' placeholder='username'>
      </p>
      <p>
        <input type='password' name='password' placeholder='password'>
      </p>
      <p>
        <input type='submit'>
      </p>
    </form>
  `
  res.send(output);
})
app.post('/auth/login', function(req, res){
  var id = req.body.username
  var pw = req.body.password
  var user = {
    username:'heonjin',
    password:'password',
    displayName:'Heonjin Jeong'
  }
  if(id === user.username && pw === user.password){
    req.session.displayName = user.displayName //화면에 보여줄 아이디를 저장함
    res.redirect('/welcome')
  }
  else{
    res.send('Login Failed .. <a href="/auth/login">Login</a>')
  }
})
app.get('/welcome', function(req, res){
  if(req.session.displayName){ // 저장된 값이 없다 == 로그인하지 않고 직접 접근했을때
    var output = `
      <h1>Hello, ${req.session.displayName}</h1>
      <a href='/auth/logout'>Logout</a>
    `
  }
  else{
    var output= `
      <h1>Welcome</h1>
      <a href='/auth/login'>Login</a>
    `
  }
  res.send(output)
})
app.get('/auth/logout', function(req, res){
  delete req.session.displayName //JavaScript의 명렁어를 사용해서 지음
  res.redirect('/welcome')
})
//
```
