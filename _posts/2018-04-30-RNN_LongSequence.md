---
layout : default
title : "RNN_Long_Sequence"
tags : Algorithm
---

## RNN Long Sequence

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [RNN Long Sequence이란?](#rnn)
2. [구현](#train)


<div id="rnn">
<h2>RNN이란?<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

```hihello``` 짧은 문장에서는 문제가 없지만

책이나 긴 글 같은 경우에는 각각의 문자를 직접 입력할 수는 없습니다.

그래서 자동으로 입력받아 학습을 진행하도록 하겠습니다.

<div id="train">
<h2>구현<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

코드를 보며 설명하겠습니다.

간단한 두줄만으로 **Dictionary** 해보겠습니다.

```{python}
sample = "if you want you"
idx2char = list(set(sample)) #index to char
char2idx = {c:i for i, c in enumerate(idx2char)} #char to index
```

<br>

**X와 Y의 Data**를 설정하는 방법을 알아보겠습니다.

```{python}
sample_idx = [char2idx[c] for c in sample]
x_data = [sample_idx[:-1]] #처음 부터 마지막-1 까지
y_data = [sample_idx[1:]] #처음 +1 부터 마지막 까지
```

X Data 는 ```if you want yo``` ,
Y Dtat 는 ```f you want you``` 가 됩니다.

<br>

**One Hot Encoding**을 하는 방법을 알아보겠습니다.

```{python}
X = tf.placeholder(tf.int32, [None, sequence_length])
Y = tf.placeholder(tf.int32, [None, sequence_length])
x_one_hot = tf.one_hot(X, num_classes)
y_one_hot = tf.one_hot(Y, num_classes)
```

**one_hot** 이라는 간단한 함수가 있으니 사용하시면 됩니다.

이 함수는 **Dimention**의 변화가 있으니 주의하여 사용합니다.

<br>

**Hyper Parameters**도 설정해보겠습니다.

```{python}
dic_size = len(char2idx)
rnn_hidden_size = len(char2idx)
numclasses = len(char2idx)
batch_size = 1 #자유롭게 변경가능
sequence_length = len(sample) - 1
```

어렵지 않습니다.

각각이 의미하는 바가 무엇인지 안다면 왜 저렇게 이루어지는지 쉽게 알 수 있습니다.

Sequence Length는 ```if you want yo``` 이므로 -1을 해주었습니다.

<br>

이제 학습을 진행하는 것은 이전과 동일하게 진행하면 됩니다.

코드는 하단과 같습니다.

```{python}
import tensorflow as tf
import numpy as np
tf.set_random_seed(777)  # reproducibility

sample = " if you want you"
idx2char = list(set(sample))  # index -> char
char2idx = {c: i for i, c in enumerate(idx2char)}  # char -> idex

# hyper parameters
dic_size = len(char2idx)  # RNN input size (one hot size)
hidden_size = len(char2idx)  # RNN output size
num_classes = len(char2idx)  # final output size (RNN or softmax, etc.)
batch_size = 1  # one sample data, one batch
sequence_length = len(sample) - 1  # number of lstm rollings (unit #)
learning_rate = 0.1

sample_idx = [char2idx[c] for c in sample]  # char to index
x_data = [sample_idx[:-1]]  # X data sample (0 ~ n-1) hello: hell
y_data = [sample_idx[1:]]   # Y label sample (1 ~ n) hello: ello

X = tf.placeholder(tf.int32, [None, sequence_length])  # X data
Y = tf.placeholder(tf.int32, [None, sequence_length])  # Y label

x_one_hot = tf.one_hot(X, num_classes)  # one hot: 1 -> 0 1 0 0 0 0 0 0 0 0
cell = tf.contrib.rnn.BasicLSTMCell(
    num_units=hidden_size, state_is_tuple=True)
initial_state = cell.zero_state(batch_size, tf.float32)
outputs, _states = tf.nn.dynamic_rnn(
    cell, x_one_hot, initial_state=initial_state, dtype=tf.float32)

# FC layer
X_for_fc = tf.reshape(outputs, [-1, hidden_size])
outputs = tf.contrib.layers.fully_connected(X_for_fc, num_classes, activation_fn=None)

# reshape out for sequence_loss
outputs = tf.reshape(outputs, [batch_size, sequence_length, num_classes])

weights = tf.ones([batch_size, sequence_length])
sequence_loss = tf.contrib.seq2seq.sequence_loss(
    logits=outputs, targets=Y, weights=weights)
loss = tf.reduce_mean(sequence_loss)
train = tf.train.AdamOptimizer(learning_rate=learning_rate).minimize(loss)

prediction = tf.argmax(outputs, axis=2)

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    for i in range(50):
        l, _ = sess.run([loss, train], feed_dict={X: x_data, Y: y_data})
        result = sess.run(prediction, feed_dict={X: x_data})

        # print char using dic
        result_str = [idx2char[c] for c in np.squeeze(result)]

        print(i, "loss:", l, "Prediction:", ''.join(result_str))
```

출력결과는 하단과 같습니다.

```
0 loss: 2.35377 Prediction: uuuuuuuuuuuuuuu
1 loss: 2.21383 Prediction: yy you y    you
2 loss: 2.04317 Prediction: yy yoo       ou
3 loss: 1.85869 Prediction: yy  ou      uou
4 loss: 1.65096 Prediction: yy you  a   you
5 loss: 1.40243 Prediction: yy you yan  you
6 loss: 1.12986 Prediction: yy you wann you
7 loss: 0.907699 Prediction: yy you want you
8 loss: 0.687401 Prediction: yf you want you
9 loss: 0.508868 Prediction: yf you want you
10 loss: 0.379423 Prediction: yf you want you
11 loss: 0.282956 Prediction: if you want you
12 loss: 0.208561 Prediction: if you want you
```

이로써 잘 학습되었다는 사실을 알 수 있습니다.

더 긴 문장도 단순히 삽입하기만 하면 됩니다.

물론 학습하는 횟수도 늘려줘야합니다.
