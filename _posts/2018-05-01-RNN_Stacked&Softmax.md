---
layout : default
title : "RNN_Stacked&Softmax"
tags : Algorithm
---

## RNN Stacked & Softmax

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [RNN Stacked & Softmax란?](#rnn)
2. [구현](#train)


<div id="rnn">
<h2>RNN Stacked & Softmax란?<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

길고 복잡한 형태의 문장은 잘 안됩니다.

그 이유는 Cell이 너무 작기 때문입니다.

Wide and Depp하게 하기 위하여 RNN을 쌓기(Stacked) 하였습니다.


<div id="train">
<h2>구현<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

첫번째는 Cell을 Stacked 하는 방법입니다.

cell = rnn.BasicLSTMCell(hidden_size, state_is_tuple=True)
cell = rnn.MultiRNNCell([cell]*2, state_is_tuple=True)

**[cell] * n** 에서 n은 몇개나 쌓을지를 결정하는 것입니다.

<br>

두번째는 Softmax를 붙이는 방법입니다.

```
X_for_softamx = tf.reshape(outputs, [-1, hidden_size])

softmax_w = tf.get_variable("softmax_w", [hidden_size, num_classes])
softmax_w = tf.get_variable("softmax_b", [num_classes])

```

**-1**은 자동으로 설정하는 것입니다.

<br>

```
outputs = tf.matmul(X_for_softmax, softmax_w) + softmax_b
outputs = tf.reshape(outputs, [batch_size, seq_len, num_classes])
```

인자로 들어가는 **outputs**는 Softmax의 결과로 나온값입니다.

또한 첫번째 outputs는 Activation function을 거치지 않았으므로

Softmax를 거친 두번째 outputs를 Sequence Loss 함수의 인자로 들어가야합니다.

<br>

나머지 학습과정은 동일합니다.

모두 종합한 코드는 다음과 같습니다.

```{python}
from __future__ import print_function

import tensorflow as tf
import numpy as np
from tensorflow.contrib import rnn

tf.set_random_seed(777)  # reproducibility

sentence = ("if you want to build a ship, don't drum up people together to "
            "collect wood and don't assign them tasks and work, but rather "
            "teach them to long for the endless immensity of the sea.")

char_set = list(set(sentence))
char_dic = {w: i for i, w in enumerate(char_set)}

data_dim = len(char_set)
hidden_size = len(char_set)
num_classes = len(char_set)
sequence_length = 10  # Any arbitrary number
learning_rate = 0.1

dataX = []
dataY = []
for i in range(0, len(sentence) - sequence_length):
    x_str = sentence[i:i + sequence_length]
    y_str = sentence[i + 1: i + sequence_length + 1]
    print(i, x_str, '->', y_str)

    x = [char_dic[c] for c in x_str]  # x str to index
    y = [char_dic[c] for c in y_str]  # y str to index

    dataX.append(x)
    dataY.append(y)

batch_size = len(dataX)

X = tf.placeholder(tf.int32, [None, sequence_length])
Y = tf.placeholder(tf.int32, [None, sequence_length])

# One-hot encoding
X_one_hot = tf.one_hot(X, num_classes)
print(X_one_hot)  # check out the shape


# Make a lstm cell with hidden_size (each unit output vector size)
def lstm_cell():
    cell = rnn.BasicLSTMCell(hidden_size, state_is_tuple=True)
    return cell

multi_cells = rnn.MultiRNNCell([lstm_cell() for _ in range(2)], state_is_tuple=True)

# outputs: unfolding size x hidden size, state = hidden size
outputs, _states = tf.nn.dynamic_rnn(multi_cells, X_one_hot, dtype=tf.float32)

# FC layer
X_for_fc = tf.reshape(outputs, [-1, hidden_size])
outputs = tf.contrib.layers.fully_connected(X_for_fc, num_classes, activation_fn=None)

# reshape out for sequence_loss
outputs = tf.reshape(outputs, [batch_size, sequence_length, num_classes])

# All weights are 1 (equal weights)
weights = tf.ones([batch_size, sequence_length])

sequence_loss = tf.contrib.seq2seq.sequence_loss(
    logits=outputs, targets=Y, weights=weights)
mean_loss = tf.reduce_mean(sequence_loss)
train_op = tf.train.AdamOptimizer(learning_rate=learning_rate).minimize(mean_loss)

sess = tf.Session()
sess.run(tf.global_variables_initializer())

for i in range(500):
    _, l, results = sess.run(
        [train_op, mean_loss, outputs], feed_dict={X: dataX, Y: dataY})
    for j, result in enumerate(results):
        index = np.argmax(result, axis=1)
        print(i, j, ''.join([char_set[t] for t in index]), l)

# Let's print the last char of each result to check it works
results = sess.run(outputs, feed_dict={X: dataX})
for j, result in enumerate(results):
    index = np.argmax(result, axis=1)
    if j is 0:  # print all for the first result to make a sentence
        print(''.join([char_set[t] for t in index]), end='')
    else:
        print(char_set[index[-1]], end='')
```

출력결과는 다음과 같습니다.

```
0 167 tttttttttt 3.23111
0 168 tttttttttt 3.23111
0 169 tttttttttt 3.23111
…
499 167  of the se 0.229616
499 168 tf the sea 0.229616
499 169   the sea. 0.229616
g you want to build a ship, don't drum up people together to collect wood and don't assign them tasks and work, but rather teach them to long for the endless immensity of the sea.
```

원래 문장과 거의 같게 출력됩니다.