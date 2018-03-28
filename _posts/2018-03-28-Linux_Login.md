---
layout : default
title : "Linux-Authentication"
tags : LinuxUnix
---

## Linux Authentication

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [/etc/passwd](#passwd)
2. [User Account User Password](#uaup)
3. [UID GID](#uidgid)
4. [Comment HomeDirectory LoginShell](#anyelse)
5. [/etc/shadow](#shadow)


<div id="passwd">
<h2>/etc/passwd<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

Login의 request(요청)을 `/etc/passwd`의 필드와 비교합니다.

먼저 /etc/passwd의 필드를 알아 보겠습니다.

```
UserAccount : UserPassword : UserID : GroupID : Comment : HomeDirectory : LoginShell
```

로 이루어져 있습니다.

그럼 이제 필드를 하나씩 알아보도록 하겠습니다.


또한 Login Shell을 통한 악성 쉘이 실행될 수 있으므로 주의를 기울여야 합니다.

<div id="uaup">
<h2>User Account User Password<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

User Account는 이름, 혹은 아이디 입니다.

마치 저의 이름 정헌진처럼 계정을 생성할 대 지정하는 것입니다.

User Password는 비밀번호 입니다.

하지만 대부분의 경우 평문으로 저장되어 있지 않습니다.

평문으로 저장될 경우 노출되기 너무 쉽기 때문입니다.

`*`처럼 별표 처리 되어있습니다.

그리고 진짜 암호는 `/etc/shadow`라는 파일에 저장되어 있습니다.

해당 파일에 대한 내용은 추후에 다루도록 하겠습니다.

<div id="uidgid">
<h2>UID GID<div style="float:right"><a href="#index">목차</a></div></h2>
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

