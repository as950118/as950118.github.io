---
layout : default
title : NodeJS-Multer
tags : NodeJS
---

## Multer

Multer란 파일을 업로드하기위한 모듈이다.
먼저 인스톨을 해보자
  
1. cmd 창을 킨다.
2. npm install multer --save 을 입력
3. 코드에 require('multer')를 통해 추가한다.
  
enctype='multipart/form-data'는 파일을 업로드하기위한 일종의 규칙이다.

middle ware인 upload.single('avatar')가 function 전에 실행하여 req라는 객체 안에 file이라는 프로퍼티가 포함되게됨.

왜 복잡한 형태를 이루는가?
더 다양한 기능을 수행가능
ex) 이미지는 uploads/img에 저장하고 텍스트는 uploads/text에 저장
ex) 같은 이름의 파일이 존재한다면 다른 이름으로 저장

/user/파일명 으로 접속하면 파일을 볼 수 있음.

app.js

```{javascript}
var multer = require('multer')//file upload
var _storage = multer.diskStorage({
  destination : function(req, file, cb){
    cb(null, 'uploads/')
  },
  filename : function(req, file, cb){
    cb(null, file.originalname)
  }
})
var upload = multer({storage:_storage})


//Upload
app.use('/user', express.static('uploads'))
app.get('/upload', function(req, res){
  res.render('upload')
})
app.post('/upload', upload.single('avatar'), function(req, res){
  console.log(req.file)
  res.send('Uploaded : ' + req.file.originalname)
})
//
```

upload.pug

```{html}
doctype html
html
  head
    meta(charset='tuf-8')
  body
    form(actoin='upload' method='post' enctype='multipart/form-data')
      br
      input(type='file' name='avatar')
      br
      input(type='submit')

```