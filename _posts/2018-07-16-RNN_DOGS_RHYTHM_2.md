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
4. [Data Set](#data)
5. [Logistic Classification](#lsc)
6. [Multiclass Classification](#mcc)
7. [LSTM](#lstm)


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

**[체온, 기온, 심장박동수, 보폭]** 5가지 요인으로

1. 이상의 유무를 가리는 Logistic Classification을 진행하겠습니다.
	여기서는 **Relu** / **Backpropagation** / 그리고 Overfitting을 방지하기위해 **Ensemble**을 이용하도록 하겠습니다.
    
2. 그 후 이상이 있다고 판정될 경우
	해당 데이터를 이용해 어떤 문제가 있을 가능성이 높은지를 예측하는 
	Multiclass Classification을 진행하겠습니다.
	여기서도 위와같은 방식을 이용하도록 하겠습니다.
    
3. 더불어 강아지의 위 5가지 요인들의 변화추이를 통해
	어떤 문제가 발생할 수 있는지를 예측하는
	여기서는 RNN LSTM을 이용하도록 하겠습니다.

그러면 하나씩 살펴보도록 하겠습니다.

<div id="data">
<h2>Data Set<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

**[체온, 기온, 심장박동수, 보폭]**를 가지는 Data Set을 생성하겠습니다.

모두 랜덤하게 생성할 수도 있지만 그럴 경우 4가지 요인의 상관성이 나타나지 않습니다.

이렇게 될 경우 모델링이 잘 됐는지를 확인할 수 없습니다.

떄문에 대략적으로 우선 체온(30~40)과 기온(-5~35)을 랜덤하게 생성한 뒤,

체온이 32~38 사이에 안들어오거나, 
특정 기온대에서 체온이 비정상적으로 높거나 낮을 경우(예를 들어 영하 5도에서 체온이 38도 정도로 높거나 영상 35도에서 체온이 32도 정도로 낮거나) 
심장박동수를 크거나 작은 범위(50~70 / 130~150)에서 랜덤하게 생성하게 하였습니다.

또한 보폭수는 별개의 요인으로 삼아서
급격하게 보폭 크기가 작거나 넓어지고
그 패턴이 주기적으로 이어진다면 안내견의 걸음걸이에 이상이 있는 것으로 판단하기로 하였습니다.

<div id="lsc">
<h2>Logistic Classification<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

그러면 우선 체온에 관한 분류를 진행하도록 하겠습니다.


<div id="mcc">
<h2>Multiclass Classification<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---


<div id="lstm">
<h2>LSTM<div style="float:right"><a href="#index">목차</a></div></h2>
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

