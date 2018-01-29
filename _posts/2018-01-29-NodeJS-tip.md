## NodeJs 자동 재시작

localhost를 이용해 nodejs를 실행하고 수정하는 작업이 반복적으로 요구된다.

이럴때 저장하고 다시 실행하는 작업은 너무 비효율적이다.

npm에서 supervisor라는 모듈을 설치하자.

콘솔을 켜보자

npm이 설치된 폴더로 이동해 npm install supervisor -g를 입력하자.

그후 node '파일명'이 아닌 supervisor '파일명'을 이용하면 된다.

