##PAPER---【RankNet】Learning to Rank using Gradient Descent
microsoft<br>

##### Introduction
------------
ranknet是使用神经网络建模出的ranking方法。

使用的训练数据是人工标注的，来表示“非常好”，“好”，“差”，“非常差”等。

目的是训练出一个可以将pair映射成real的function，这样就把一个ranking问题转化成一个regression函数。

使用NN，也是其具有很强的灵活性。因为两层NN可以拟合任意有界的连续函数。

##### Previous Work
------------
简单地介绍RankProp,PRank,RankBoost，指出RankProp,PRank的不足之处，以及和RankBoost的不同。


