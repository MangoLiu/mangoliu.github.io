##Book---Learning to Rank for Information Retrieval (2)
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
**SVM-Based Method**用于二分类，简单的说是就是在N维的空间中使用N-1w维的超平面来做分割。不妨先假设为线性函数：f(w,x) = wT*x。SVM的形式如下：<br>
![lty_22](/images/liutieyan/lty_22.png)<br>
SVM使用的是hinge loss，即分类正确时loss为0,不正确时为线性惩罚。hinge loss的形式如下：<br>
![lty_23](/images/liutieyan/lty_23.png)<br>
图中的hinge loss 的yj取值是{+1，-1}。<br>
SVM通过使用kernel trick来处理非线性case。<br>

注：关于SVM参考[支持向量机通俗导论（理解SVM的三层境界）](http://blog.csdn.net/v_july_v/article/details/7624837)。


**Logistic Regression-Based Method**逻辑斯特回归常用于二分类，虽然它的名字中含有regression。<br>
对于document xj 它的几率的对数形式为：<br>
![lty_24](/images/liutieyan/lty_24.png)<br>
接下来，它可以转换成：<br>
![lty_25](/images/liutieyan/lty_25.png)<br>
参数wt可以通过最大似然估计来估算。<br>

**Boosting Tree-Based Method**是多分类器。pass<br>



#### 2.4 OrdinalRegression-Based Algorithms
------------------


#### 2.5 Discussions and Summary
------------------



