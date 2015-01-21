#LTR---RankNet

ranknet神经网络在rank上的一种实现。

训练数据是一些query/doc pair，根据关联性进行多label打分。因为在rank中，我们更加注重doc之间的比较关系，而非doc作为一个单独的个体的rank value。即ranknet是一种pairwise approach。

为了后面的描述，我们先做假设:label共分为N个等级，有m行训练数据，每行数据有n维特征。









参考：<br>
1《Learning to Rank using Gradient Descent》<br>
2 《Learning to Rank for Information Retrieval》<br>

--------------------------------
######（转载本站文章请注明作者和出处 <a href="https://github.com/MangoLiu">MangoLiu</a> ，请勿用于任何商业用途）

