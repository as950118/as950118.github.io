---
layout : default
title : "RNN_DOGS_RHYTHM"
tags : Algorithm
---

## RNN Dogs Rhythm

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [RNN Dogs Rhythm이란?](#rnn)
2. [문제점](#problem)
3. [구현](#train)


<div id="rnn">
<h2>RNN Dogs Rhythm이란?<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

LSTM RNN을 이용할 것입니다.

강아지의 체온, 기온, 심장박동수, 보폭 등의 다양한 요인들을 이용하여

강아지의 신체에 이상이 있는지, 있다면 어떤 이상인지를 분석하는 작업을 진행하겠습니다.

주가 예측을 한다고 생각하시면 될 것 같습니다.

<div id="problem">
<h2>문제점<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

문제점이 있습니다.

데이터가 없다는 것입니다.

그래서 우선은 가상의 데이터를 통하여

모델링만을 완성한 후에

실제 데이터를 구하는 작업을 실행하도록 하겠습니다.


<div id="train">
<h2>구현<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

먼저 LSTM의 구조를 살펴보겠습니다.

LSTM은 크게 One to One / Many to One / Many to Many

세가지 모델이 존재합니다.

이중에서 지금 이용할 모델은 Many to Many입니다.

Input Data Vector Length는 

[체온, 기온, 심장박동수, 보폭]으로 **5** 이고

Input Data Sequence Length는

우선은 **100**개로 하겠습니다.

Output Data Vector Length는

