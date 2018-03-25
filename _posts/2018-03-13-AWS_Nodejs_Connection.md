---
layout : default
title : "AWS_NodeJS_Connection"
tags : Nodejs
---

# AWS NodeJS 연동

---

먼저 AWS의 인스턴스에 접속해줍니다.

그리고 콘솔창에서 진행하겠습니다.

1.업데이트를 해줍니다.

```
sudo apt-get update
```

2.nodejs를 설치해줍니다

```
sudo apt-get install nodejs
```

3.npm을 설치해줍니다.

```
sudo apt-get install npm
```

4.express를 설치해줍니다.

```
sudo npm install -g express
```

5.exporess generator 4버젼을 설치합니다

```
sudo npm insatll -g express-generator@4
```

6.node 모니터링을 위한 nodemon을 설치합니다.

```
sudo npm install -g nodemon
```

7.테스트를 진행할 디렉터리를 만들고 이동합니다.

```
sudo mkdir testnodejs && cd testnodejs
```

8.nodejs 디렉터리를 node로 수정합니다.

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

9.테스트용 기본 템플릿을 생성합니다. 뒤에 --view=ejs는 뷰 엔진으로 ejs를 사용한다는 의미입니다.

```
sudo express -e
```

10.node packages를 설치합니다.

```
sudo npm install
```

11.app.js를 편집하겠습니다.

```
sudo vi app.js
```

```{javascript}
app.listen(3000, function(){
  console.log('Coneccted 3000 port')
})
```

12.package.json을 편집하겠습니다.

```
sudo vi packge.json
```

```
"start" : "nodemon app.js"
```

13.이제 서버를 실행하겠습니다.

```
npm start
```

잘 연결되면 **Connected 3000 port**가 콘솔에 나타납니다.

14.이제 백그라운드에서 실행하는 방법에 대해 알아보겠습니다.

백그라운드에서 실행시키기 위해서 nohup을 사용합니다.

```
nohup npm start &
```

잘 실행되면 process id가 나타날 것 입니다.

이제 app.js로 접속하였던 주소를 새로고침하여도 연결이 끊어지지 않습니다.

15.추후에 백그라운드를 종료하기위해서는 하단처럼 입력합니다.

```
ps -ef
```

를 통해 node app.js에 해당하는 process id를 찾아낸후

```
kill process id
```

16.이제 port redirection을 해보겠습니다.

지금처럼 뒤에 포트넘버가 나오는 형태는 너무 더럽습니다.

그를 위해서는 80번 포트를 3000번 포트로 자동 리다이렉션 합니다.

```
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```

이제 포트넘버를 입력안해도 자동으로 3000으로 이동하게됩니다.


17.어떤 포트에 연결되었는지 확인해봅시다

```
netstat -tnlp
```

