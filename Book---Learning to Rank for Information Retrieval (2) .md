##Book---Learning to Rank for Information Retrieval (1)
Tie-Yan Liu  Microsoft Research Asia<br>
### The Pointwise Approach
#### 2.1 Overview
------------------
使用传统机器学习方式进行ranking最直接的方式就是pointwise方法， 预测documet的relevance degree，之后进行ranking，虽然目标是ranked list。<br>

pointwise 大体包含了三类算法： regression-based algorithms, classification-based algorithms,
and ordinal regression-based algorithms.<br>

regression-based的输出是以数值表示的value；classification-based的输出是无序的分类；而ordinal regression-based的输出是有序的分类。<br>


#### 2.2 Regression-Based Algorithms
------------------
Regression-Based Algorithms是把ranking问题看作回归问题，预测一个连续值。<br>
经常使用的loss function是square loss：<br>
![lty_2](/images/liutieyan/lty_2.png)<br>
square loss的图像如下：<br>
![lty_21](/images/liutieyan/lty_21.png)<br>
带权重的回归模型可以更好的关注那些分类错误的doc，例如在搜索引擎中那些排在高位的结果。<br>

#### 2.3 Classification-Based Algorithms
------------------
基于分类的方法和基于回归的方法有所不同，它的输出是有限的label名称，而不是一个实数值。<br>
**SVM-Based Method**用于二分类，不妨先假设为线性函数：f(w,x) = wT*x。SVM的形式如下：<br>
![lty_22](/images/liutieyan/lty_22.png)<br>
SVM使用的是hinge loss，即分类正确时loss为0,不正确时为线性惩罚。hinge loss的形式如下：<br>
![lty_23](/images/liutieyan/lty_23.png)<br>


注：关于SVM详见机器学习---SVM.
#### 2.4 OrdinalRegression-Based Algorithms
------------------


#### 2.5 Discussions and Summary
------------------



