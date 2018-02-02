---
layout : default
tags : NodeJS
---
## post 방식

하단의 코드를 보면서 이해해보자

```{JavaScript}
app.post('/topic', function(req, res){
  var title = req.body.title
  var description = req.body.description
  fs.writeFile('data/'+title, description, function(err){//fs에 관한 것은 nodejs api에서 검색
    if(err){
      res.status(500).send('Internal Server Error')//error 코드
      console.log(err)
    }
      res.send('Connected router ')//잘 연결될때
  })
})
```

