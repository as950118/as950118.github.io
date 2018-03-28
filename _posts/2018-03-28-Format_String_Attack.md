---
layout : default
title : "Format_String_Attack"
tags : LinuxUnix
---

## Format String Attack

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [개념](#intro)
2. [코드](#code)
3. [UID GID](#uidgid)
4. [Comment HomeDirectory LoginShell](#anyelse)
5. [/etc/shadow](#shadow)


<div id="intro">
<h2>개념<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

c 언어의 **printf**나 자바의 **System.out.format**에서의 취약점을 통해

메모리 내용 변조 혹은

원하는 REt 영역으로 이동후 악성코드가 위치한 주소로 변조하는 방식의 공격입니다.

<div id="code">
<h2>코드<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

```{c}
#include<stdio.h>

int main(int argc, char **argv){
	
	char *name = "Jeong"
	int age = 25;
	int nbytes = 0;
	
	printf("Name : %s age = %d %n \n", name, age, &nbytes);//\n이 아니라 %n임 
	printf("nbytes : %d", nbytes);//"Name ~~ age = 25" 까지의 문자열 길이가 출력됨 
	
	return 0;
}
```


<div id="uidgid">
<h2>코드 해석<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

모든 필드가 중요하지만

그중 특히 **UID**(UserID), **GID**(GroupID)는 해당 계정의 권한을 결정하므로 중요합니다.

만약 UID와 GID가 **0** 이라면 **root** 계정입니다. (Windows에서는 **500**)


<div id="anyelse">
<h2>Comment HomeDirectory LoginShell<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

**Commnet**는 root, bin 처럼 계정에 대한 설명을 나타냅니다.

**Home Directory**는 /root, /bin 처럼 각 계정의 디렉터리 위치를 나타냅니다.

**Login Shell**은 bash, nologin 처럼 각 계정의 종류에 따라 달라집니다.

이중 **bash**는

```
/etc/profile =>
~/.bash_profile or ~/.bashrc_login or ~/.profile =>
~/.bashrc =>
/etc/bashrc
```

순서로 실행된 결과로

가장 기본적인 shell 형태로

GNU 프로젝트를 위해 만들어져서

쉘 형태로 명령어 실행을 가능하게 합니다.

이외에도 ksh, zsh 등 다양한 쉘이 존재합니다.

**nologin**은 **ftp**(File Transfer Protocol)만 가능한 형태입니다.


<div id="shadow">
<h2>/etc/shadow<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

앞서 말했듯이

/etc/passwd의 UserPassword 필드는 /etc/shodow에 저장됩니다.

그럼 먼저 이 파일을 보겠습니다.

파일은

`$<ID>$<Salt>$<Encrypeted_password>`

로 구성되어 있습니다.

각각의 필드르 보면

**ID**는 어떠한 암호화 알고리즘을 이용할 것인지를 결정하는 부분입니다.

1 = MD5, 2 = Blowfish, 5 = SHA-256, 6 = SHA-512

입니다.

**Salt**는 암호화를 좀 더 복잡하게 해주는 임의의 값입니다.

**Encrypted Password**는 ID와 Salt를 통해 암호화된 암호 결과값입니다.

