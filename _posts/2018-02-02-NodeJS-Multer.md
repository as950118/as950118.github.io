---
layout : default
title : NodeJS-Multer
tags : NodeJS
---

# Multer
---

Multer란 파일을 업로드하기위한 모듈이다.

## 설치
  
1. cmd 창을 킨다.
2. npm install multer --save 입력  


## 코드
  
1. 하단의 코드를 추가한다.

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
    form(actoin='upload' method='post'sd enctype='multipart/form-data')
      br
      input(type='file' name='avatar')
      br
      input(type='submit')

``````

## 코드 해석


1. require
2. enctype='multipart/form-data'는 파일을 업로드하기위한 일종의 규칙이다.

3. middle ware인 upload.single('avatar')가 function 전에 실행하여 req라는 객체 안에 file이라는 프로퍼티가 포함되게됨.

## 설명

1. 왜 복잡한 형태를 이루는가?
	더 다양한 기능을 수행가능
	ex) 이미지는 uploads/img에 저장하고 텍스트는 uploads/text에 저장
	ex) 같은 이름의 파일이 존재한다면 다른 이름으로 저장

/user/파일명 으로 접속하면 파일을 볼 수 있음.

