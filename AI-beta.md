#AI-beta
web资源收集，主要在LTR paper摘要

###《Learning to Rank using Gradient Descent》Chris Burges
Ranknet的经典之作。 <br>
* 采用梯度下降方法。
* 本质上是神经网络的一种实现。

* RankProp也是一种神经网络排序模型，它交替使用MSE regression和对当前排序结果的调整。它的输出是对期待排序的结果，而非具体的量化值。它的优点是作用于单独个体，而不是pair，但是我们不知道它在何种情况下收敛。

* PRank将特征向量转换为实数，学习时每次使用一个实例。
PRank是一种pair-based算法，需要在O(m^2)pairs而不是O(m)。
* RankBoost也是基于pairs来训练的，boosting也可以被视作一种梯度下降。