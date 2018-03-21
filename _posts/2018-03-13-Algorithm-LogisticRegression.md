---
layout : default
title : "A-Logistic_Classification"
tags : Algorithm
---

## Logistic_Classification(로지스틱 분류)

---

<div id="index">
<h2>목차</h2>
</div>

---

1. [개념](#intro)
2. [선형 회귀분석](#linear)
3. [로지스틱 회귀분석](#logistic)
4. [코드](#code)
5. [결론](#conclusion)


<div id="intro">
<h2>개념<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

분류 문제 중 분류가 예, 아니오 처럼 **2가지로만** 나누어진 경우 **이항형**에 사용하는 알고리즘 입니다.

>분류문제란 새로운 데이터가 어느 그룹 속하는지를 구분하는 문제입니다.
>
>그 중 이항형 문제는 Logistic Regression으로,
>
>다항형 문제는 Multinomual Logistic Regression, 혹은 Ploytomous Logistic Regression을 이용하고,
>
>다항형 문제 중 순서가 존재하는 경우에는 Orrdinal Logistic Regression을 이용합니다.
>
>오늘은 이항형의 경우만 다루겠습니다.

그럼 그에 앞서 **선형 회귀분석**에 대해 설명하겠습니다.

<div id="linear">
<h2>선형 회귀분석<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

y= ax + b (x,y : -Inf<=x,y<=Inf(즉, 무한대)) 형태의 **1차 함수**를 분석하는 것입니다.

>분석을 위해 이 **a와 b**를 찾아내야 하는데,
>
>그를 위한 방법으로는 **최소제곱법**이 사용됩니다.

다시말해 x의 변화에 따라 y가 어떻게 변하는지, 혹은 y의 변화에 따라 x가 어떻게 변하는지,

즉 x와 y가 어떤 관계를 가지는지 예측하는 것입니다.

<div id="logistic">
<h2>로지스틱 회귀분석<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

선형 회귀분석의 개념에서 착안하여 **y = A일 확률**이라고 정의하고 **y >= a**이면 A, **y < a**이면 not A(=B)라고 분류하는 것입니다.
>
>**a**를 적절히 정하는 것이 매우 중요합니다.

그래서 먼저 **y의 범위**를 수정해줘야 합니다.

기존의 범위에서는 -Inf to Inf지만 확률은 **0 to 1**를 가져야하기 때문입니다.

관계식은 **P = e^(ax + b) / 1 + e^(ax + b)**입니다.

![Not so big](../assets/img/Algorithm/Tensorflow/losistic.png)

>y = ax + b
>**y를 P로 수정**
>P = ax + b
>>P:0<=P<=1이므로 성립하지 않음
>
>**P를 Odds로 수정**
>Odds = P / (1-P) = ax + b
>>Odds:0<=Odds<=Inf이므로 성립하지 않음
>
>**Odds에 log_e를 씌움**
>log_e(Odds) = log_e(P / 1-P) = ax + b
>>log_e(Odds):-Inf<=log_e(Odds)<=Inf이므로 성립함
>
>**e를 씌움**
>e^{log_e(P / {1-P})} = e^(ax + b) = P / (1-P) = e^(ax + b)
>
>
>**역수를 취함**
>1-P / P = 1 / e^(ax + b)
>
>
>**+1을 함**
>1 / P = {1 / e^(ax + b)} +1 = {1 + e^(ax + b)} / e^(ax + b)
>
>
>**역수를 취함**
>P = e^(ax + b) / 1 + e^(ax + b)


이러한 함수는 완전한 형태는 아닙니다.

그래서 log를 씌워서 저희가 원하던, 직선 형태로 바꿉니다.

![Not so big](../assets/img/Algorithm/Tensorflow/LogHX.png)

그리고 이 최적점을 찾는 것은 
Cost(W) = (1/m)∑c(H(x),y)

= C(H(x),y) = -log(H(x))  (y=1), -log(1-H(x)) (y=0)

즉 **-ylog(H(x))-(1-y)log(1-H(x))**

형태로 표현될 수 있습니다.

이것을 Tensorflow를 이용하여 구현해보겠습니다.

<div id="code">
<h2>코드<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

```{python}
import tensorflow as tf

x_data = [[1,2],[2,3],[3,1],[4,3],[5,3],[6,2]]
y_data = [[0],[0],[0],[1],[1],[1]]

X = tf.placeholder(tf.float32, shape=[None, 2])
Y = tf.placeholder(tf.float32, shape=[None, 1])
W = tf.Variable(tf.random_normal([2,1]), name='weight')b = tf.Variable(tf.random_normal([1]), name='bias')

hypothesis = tf.sigmoid(tf.matmul(X,W)+b)

cost = -tf.reduce_mean(Y * tf.log(hypothesis) + (1 - Y) * tf.log(1 - hypothesis))
train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)

predicted = tf.cast(hypothesis>0.5, dtype = tf.float32)
accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, Y), dtype = tf.float32))

sess = tf.Session()
sess.run(tf.global_variables_initializer())

for step in range(10001):
    cost_val, _ = sess.run([cost, train], feed_dict={X:x_data, Y:y_data})
    if step % 200 == 0:
        print(step, cost_val)

h, c, a = sess.run([hypothesis, predicted, accuracy], feed_dict={X:x_data, Y:y_data})
print("\nhypothesis : ", h, "\ncorrect : ", c, "\naccuracy : ", a);

```

![Not so big](../assets/img/Algorithm/Tensorflow/LogisticClassification_binary.png)

각각의 코드를 보겠습니다.

```{python}
X = tf.placeholder(tf.float32, shape=[None, 2])
Y = tf.placeholder(tf.float32, shape=[None, 1])
```

**shape**라는 것이 보입니다.

배열의 첫번째 인자는 **배열의 개수**를 의미합니다.

None이라는 것은 N, 즉 어떠한 수라도 들어올수 있다는 것입니다.

두번째 인자는 **배열의 원소배열의 크기**를 의미합니다.

예를 들자면 shape=[None, 2]라는 것은

Arr = [[1,2], [2,3], [3,4]..] 라는 배열만 들어올 수 있다는 의미입니다.

```{python}
W = tf.Variable(tf.random_normal([2,1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')
```
**W**는 **weight(가중치)**, 함수에서 보자면 y = ax + b에서 바로 이 a 입니다

**b**는 **bias(편향치)**, 함수에서 보자면 y = ax + b에서 바로 이 b 입니다.

Variable 함수는 변수를 지정한다는 것이고

random_normal 함수는 은 임의의 수를 생성하는 것입니다.

그러면 random_normal 함수의 인자를 보겠습니다.

첫번째 인자는 어떠한 크기를 가진 값으로 출력될지를 정하는 것입니다.

**[2,1]**이라는 것은 **2개의 변수**로 이루어진 **1개의 변수**로 출력된다는 것입니다.

예를 들면, [[1,2]]처럼 출력된다는 것입니다.

**[1]**은 [[1]]처럼 출력된다는 것입니다.

```{python}
hypothesis = tf.sigmoid(tf.matmul(X,W)+b)
```

**sigmoid**는 **1 / (1 + e^(ax + b))**를 의미합니다.

**matmul**은 **행렬의 곱셉**을 의미합니다.

```{python}
cost = -tf.reduce_mean(Y * tf.log(hypothesis) + (1 - Y) * tf.log(1 - hypothesis))
```

**reduce_mean**는 **평균**을 구하는 함수 입니다.[왜 reduce인가?](#reduce)

안의 수식은 위에서 변환을 통해 얻은 수식을 이용한 것입니다.

```{python}
train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
```

**train**은 본격적인 머신러닝을 수행하는 모듈입니다.

**GradientDescentOptimizer**은 **optimize(최적화)**하는 방법 중, 곡선을 따라내려가며 최적점을 찾아 가는 방식의 함수입니다.

```{python}
predicted = tf.cast(hypothesis>0.5, dtype = tf.float32)
accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, Y), dtype = tf.float32))
```

**predicted**는 hypothesis(가설)에 따라 어떤 **결과**가 나올것인지를 예측**하는 결과입니다.

**accuracy**는 가설에 따라 나온 결과와 **실제값이 얼마나 일치**하는 지를 확인하는 것입니다.

```{python}
sess = tf.Session()
sess.run(tf.global_variables_initializer())
```

**Session**은 tensorflow를 시작하는 것입니다.

**global_variables_initializer**는 변수들을 초기화 하는 것입니다.

```{python}
for step in range(10001):
    cost_val, _ = sess.run([cost, train], feed_dict={X:x_data, Y:y_data})
    if step % 200 == 0:
        print(step, cost_val)
```



h, c, a = sess.run([hypothesis, predicted, accuracy], feed_dict={X:x_data, Y:y_data})
print("\nhypothesis : ", h, "\ncorrect : ", c, "\naccuracy : ", a);


<div id="conclusion">
<h2>결론<div style="float:right"><a href="#index">목차</a></div></h2>
</div>

---

<div id="reduce"><div>
여담이지만 reduce_mean이나 sum에서 **reduce**가 붙는 이유를 이야기하자면 좀 긴데, 먼저 파이썬 내장 함수중 reduce에 대해 알아야 합니다.

reduce는 원소를 프로그래밍된 대로 순차적으로 처리하는 함수입니다.

가령 예를 들면, reduce(x,y : x+y , [a,b,c,d,e])가 있다면

edcba가 출력되는 식입니다.(b+a, c+ba, d+cba, e+dcba)

reduce(x,y : x+y , [1,2,3,4,5])면 15가 출력되겠죠.

그리고 reduce_mean,sum에는 **axis**라는 인자가 있습니다.

이것은 **어느 차원까지 합칠것인지**를 나타냅니다.

예를 들면, **reduce_mean([[1,2,3],[2,3,4]] axis=1)**은

**[2,5]**가 출력되고

이것은 곧 **shape=(2,3)**에서 **shape=(2,)**로 바뀐다는 것을 의미합니다.

사실, 차원이 축소하니깐 reduce를 붙인다 라고 이해하는 것이 쉽습니다.

---

여담이지만 sigmoid보다 ReLU를 이용하는 것도 알아보고 싶습니다.

