---
layout : default
title : "RNN_Dynamic"
tags : Algorithm
---

## RNN Dynamic

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [RNN Dynamic란?](#rnn)
2. [구현](#train)


<div id="rnn">
<h2>RNN Dynamic란?<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

RNN의 강점은 Sequence Data를 처리하기에 유리합니다.

하지만 정해지지 않은 형태로 입력이 들어올 수 있습니다.






<div id="train">
<h2>구현<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

기존의 방식에는 Padding을 넣어서 처리합니다.

하지만 이러한 방식은 오류가 발생하는 경우가 많습니다.


각각의 Batch의 Length를 정의합니다.

그러면 해당하는 길이 만큼만 값을 주고 나머지는 0으로 정의됩니다.

```{python}
outputs, _states = tf.nn.dynamic_rnn(cell, x_data, sequence_length=[5,3,4], dtype=tf.float32)
```