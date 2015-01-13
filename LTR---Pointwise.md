#LTR---Pointwise
问题假设：为预测的每一个doc都预测一个具体的值，虽然不是很需要。(因为在排序时，我们更注重它们的序，而对具体的值不是太关心。)<br>

大体分为三大类：regression-based algorithms, classification-based algorithms, and ordinal regression-based algorithms<br>

### Regression-Based Algorithms
与一般的回归问题一样是一个有监督的学习模型，需要设定loss function，可以使用 square loss(最好使用NDCG值来衡量)<br>

但要注意的是，在不同位置是要有不同权值的。由于用户是从上而下浏览的，所以排名靠前的位置要有很高权重。<br>

### Classification-Based Algorithms
#### Binary Classification for Ranking
将相关的样本作为正样本(+1)，将不相关的样本作为负样本(-1)，进行二分类。有基于SVM，也有基于逻辑回归。<br>


