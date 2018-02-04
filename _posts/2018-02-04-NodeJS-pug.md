---
layout : default
title : NodeJS-pug
tags : NodeJS
---

## pug

html 코드를 간략하고 직관적으로 입력할 수 있게 도와주는 모듈이다.

기존엔 jade 였지만 어떠한 법적 문제 때문에 pug라고 바꾸게 되었다.

어떤 문제에 봉착했을때 구글링에 pug로 안 나온다면, 기능은 기존과 동일하니 jade로 검색해보자.

우선 설치를 해보자.

1. cmd 창을 킨다.
2. npm init이 된 디렉터리로 이동한다.
3. npm install pug --save

설치를 했다면 pug를 이용한 파일을 하나 생성해보자.

1. test.pug 라는 파일을 생성한다.
2. 하단의 코드를 입력한다.
	```{pug}
    doctype html
    	head
        body
        	h1="Hello World"
    ```
3. 크롬 등을 이용하여 실행해보자.

pug의 더 자세한 사용법은 npm api에서 찾아보도록 하자.
