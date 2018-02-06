---
layout : default
title : NodeJS-orientDB
tags : NodeJS
---

# OrientDB를 이용한 로그인 구현
---

많은 경우에 NodeJS에는 MongoDB를 많이 연동한다.

하지만 생활코딩을 보며 OrientDB에 대한 흥미가 생겨 여기서는 해당 데이터베이스를 이용해 회원가입-로그인을 구현해 보겠다.

먼저 OrientDB를 설치해보자.

### 설치
---


### 실행
---
cmd
설치폴더이동
bin이동
server.bat

active 될 때까지 대기한다.

active가 되면 port number를 찾는다.

본인의 경우는 2480이다.

localhost:2480 로 접속하면 관리자 화면이 뜬다.

우측 상단의 ** NEW DATABASE ** 를 클릭하여 DB를 생성한다.

Name, Server User, Server Password 가 있을 것이다.

Server User는 root로 되어있을테니 Name과 Password를 정해주자.

본인이 편한 것으로 정하자.

BROWSER에서는 SQL문을 이용해 DB를 실행할 수 있다.

SCHEMA에서는 일반적으로 DB에서 말하는 table과 같은 역할을 하는 Class와 Record를 관리할 수 있다.

그러면 Schema에서 Class를 추가해보자.

Generic Class에서 ** NEW GENERIC ** 을 해보자.

이름은 진행하는 프로젝트에 맞춰 topic으로 정한다.

그리고 ** NEW PROPERTY ** 를 해보자.

가장 먼저 title이라는 이름의 property를 생성하자.

타입은 STRING으로 지정하자.

이때 가장 아래쪽을 보면 몇가지 옵션이 보일 것이다.

mandatory는 필수적으로 입력해야 하는 항목일때 지정해 주는 것이다.

제목은 필수적이어야 하므로 이 옵션을 선택해주자.

그리고 description이라는 이름의 property를 생성하자.

타입은 STRING으로 지정하자.

설명은 필수적일 필요가 없으므로 옵션을 선택하지 않는다.

그러면 이제 Class가 생성되었을 것이다.

해당 Class를 클릭해 들어가보자.

들어가보면 방금 설정한 Properties와 기타 여러가지 것들이 보일 것이다.

그러면 이제 ** NEW RECRODE ** 를 해보자.






MongdoDB의 속성에서 autoIndex가 없어짐

