---
layout : default
title : NodeJS-Get
tags : NodeJS
---

## Get 방식

web에서 data를 transport하는 방법에는 크게 2가지가 있다.

여기서는 그 중 get 방식에 대해 이야기 하도록 하겠다.

처음부터 너무 어려운 이야기를 하려면 이해가 어려울 듯 하다.
  
  
  
그래서 우선 쉽지만 조금은 부정확하게 설명해보겠다.

지금 네이버를 쳐서 무언가를 검색해보자.

검색하면 화면이 바뀌고 url도 바뀌는 것이 보일 것이다.

그 url을 잘 들여다보면 본인이 검색한 내용이 보일 것이다.

이렇게 url에 data가 노출되는 것이 get 방식이다.
  
  
  
그럼 이제 왜 url에 노출되는지에 대해 설명하겠다.

하단의 코드를 보자.

get방식으로 연결하는 코드이다.

```{JavaScript}
var express = require('express')
var app = express()
app.locals.pretty = true
app.set('view engine', 'pug')
app.set('views', './views')
app.use(express.static('public'))//public이라는 디렉터리에서 가져옴
//입력받아서 출력하기
app.get('/form', function(req, res){
  res.render('form')
})
app.get('/form_receiver', function(req, res){
  var title = req.query.title
  var description = req.query.description
  res.send(title+','+description)
})
//
//connection
app.listen(3000,function(){
  console.log('Connected on 3000 port');
})
//

app.get('/topic', function(req, res){
  res.send(req.query.id)//url을 통해 들어온 값이 첫번째 매개변수인 req의 query라는 객체의 id라는 프로퍼티값으로 들어오고 출력됨
  //res.send(req.query.id)으로 하면 topic?id= 뒤에 들어오는 값이 출력됨
  //res.send(req.query.id+','res.send(req.query.name))으로 하면 topic?id={}&name={}형식으로 입력받게됨
  //더 궁금한게 있다면 expressjs.com의 api에서 찾아보도록하자
})

```

하나씩 해석해보자.

require
- module을 불러오는 method이다.  
해당 코드에서는 express를 불러오고  
express method를 app에 불러왔다.

app.locals.pretty = true
- pug라는 module을 이용할 때 코드가 가독성이 좋게 출력된다.  
이에 앞서 pug에 대한 설명이 필요할 듯 하다.
[pug](.2018-02-04-NodeJs-pug.md) <- link

app.set('view engine', 'pug')
- app의 view engine을 pug로 사용하겠다는 의미이다.
[set에 관한 설명]()

app.use(express.static('public'))

