---
layout : default
tags : NodeJS
title : NodeJS-XSS
---

#Nodejs-SQL_Injection

##SQL_Injection 이란?

SQL문의 허점을 이용한 해킹 기법이다.

쉽게 예를 들어보면 `SELECT FROM db WHERE id=:A` 같은 SQL문이 있다고 가정해보자.

이런 경우 A에 `1 or 1=1`라는 값이 입력되게 되면,

`SELECT FROM db WHERE id=1 or 1=1`이 된다.

이 경우, id=1이 아니더라도 1=1은 무조건 True이므로 db에 저장되어 있는 값들을 불러올 수 있게 된다.

이 외에도 ID나 PW를 통한 접속에서도 1or 1=1이 입력되면 무조건 True값이 되므로 해당 정보에 접속할 수 있게 된다.

##해결방법

mysql같은 경우에는 해결방법이 다양하다.

우선 mysql 모듈 자체에 .escape라는 함수를 통해 쉽게 해결 가능하다.

하지만 orientdb에 관한 함수는 없는 것으로 보인다.

그렇다면 직접 한번 구현해보자

###OrientDB의 SQL-Injection 방어

사실 SQL-Injection은 그 종류와 상황이 너무도 다양하기에 완벽한 해결책은 존재하기 힘들다는 사실을 전제로하여 진행하겠다.

####개요

1. `=` `<` `>` `'` `"` 라는 문자가 들어가면 전송을 거부한다.
2. 해당 문자가 있는지 검사하기 위해 JavaScript에서 split함수를 이용한다.

##설치

mysql이라면 connection.escape를 이용한다. 자세한 사용법은 다큐먼트를 참조하도록 하자.

[Mysql DOCS](https://www.npmjs.com/package/mysql#escaping-query-values)의 Escaping query values

##코드

###Mysql

```{javascript}
var userId = 'some user provided value';
var sql    = 'SELECT * FROM users WHERE id = ' + connection.escape(userId);
connection.query(sql, function (error, results, fields) {
  if (error) throw error;
  // ...
});
```

###OrientDB

```
passport.use(new LocalStrategy(
    function(username, password, done){
      var userdata = {
        id:username,
        pw:password
      }
      var filter = userdata.id.split('')
      for(var i in filter){
      	if(filter[i]==="'"||filter[i]==='"'filter[i]==="="||filter[i]==="<"||filter[i]===">"){
        	console.log('SQL INJDECTION')
        	return done(null, false)
        }
      }
      var sql = 'SELECT * FROM log WHERE id=:id'
      db_log.query(sql, {params:{id:userdata.id}})
      .then(function(results){
        if(results.length === 0){
          return done(null, false) //fail
        }
        var user = results[0]
        return hasher({password:userdata.pw, salt:user.salt}, function(err, pass, salt, hash){ //
          if(hash === user.pw){
            done(null, user) //success
          }
          else{
            done(null, false)
          }
        })
      })
    }
  ))
```

로그인을 해보자.

ID와 PW에 1 or 1=1을 넣어서 실행해보자.

해당 코드에서는 이미 hash와 salt를 이용한 암호화를 하였고

SQL문 자체도 해당 SQL injection에 대한 취약점을 보이지 않기 때문에 저러한 코드를 삽입하지 않더라도 큰 문제점은 없다.

하지만 어떻게 방지할 수 있는지에 대해 설명하기 위함이었으므로 그러하게 이해하면 될 것이다.

##결론

OrientDB는 정보가 참 많이 부족하다.