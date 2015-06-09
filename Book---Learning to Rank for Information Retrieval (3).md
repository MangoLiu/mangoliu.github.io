##Book---Learning to Rank for Information Retrieval (3)
Tie-Yan Liu  Microsoft Research Asia<br>
### The Pairwise Approach
#### 3.1 Overview
------------------
Pairwise Approach 相关的各种算法的区别主要在于loss function是不同的。<br>
pairwise不再关注单一doc的想相关性分值，而是关注于两个doc之间的相互序。此时ranking通常就转变为对doc pair的分类，例如判断pair中那个doc更好一些。<br>
学习的目标就是使分错类的pair数目最小。<br>


#### 3.2 Example Algorithms
------------------
Regression-Based Algorithms是把ranking问题看作回归问题，预测一个连续值。<br>
经常使用的loss function是square loss：<br>
![lty_2](/images/liutieyan/lty_2.png)<br>
