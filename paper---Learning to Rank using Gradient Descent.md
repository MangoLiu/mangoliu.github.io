##RankNet
ranknet就是Microsoft的那篇[《Learning to Rank using Gradient Descent》](http://research.microsoft.com/en-us/um/people/cburges/papers/ICML_ranking.pdf?WT.mc_id=Blog_MachLearn_General_DI)最先提出的。
**目标**：得到一个算分函数f(x,w)，其中x为doc的特征，w为权重。<br>
**训练模型**：神经网络。<br>
**优化方法**：梯度下降。<br>
**损失函数**：交叉熵。<br>

在标注label的doc集合中，由于已知label，即可知道相邻两个doc构成的pair的值（1代表前者好于后者，0代表相反，0.5代表一样好），然后构造一个模型为神经网络的算分函数f(x,w)，它可以计算出每个doc的分值，利用这个分值，可以构造一个计算前者比后者好的概率的方法，该方法去估算上述那些pair的概率值，并希望拟合的尽可能好。其中使用的损失函数的是交叉熵，使用的优化方法是梯度下降。


Q:ranknet使用的训练模型，优化方法以及损失函数是什么？<br>
Q:为什么说ranknet是pairwise形式的？<br>
Q:为什么选用交叉熵来做损失函数？<br>
Q:训练时的复杂度是多少？<br>
Q:<br>
Q:<br>
Q:<br>



##### 其他资源
------------
http://blog.csdn.net/huagong_adu/article/details/40710305
http://blog.csdn.net/puqutogether/article/details/43667375
https://github.com/y-tag/cpp-ToyBox-Ranking/blob/master/lib/ranknet.cc
http://www.cnblogs.com/kemaswill/p/kemaswill.html
http://people.cs.umass.edu/~vdang/ranklib.html

http://blog.csdn.net/puqutogether/article/details/42124491