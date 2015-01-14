#AI-beta
资源收集，主要在LTR paper摘要。其中有部分是自行添加或是个人想法。

###《Learning to Rank using Gradient Descent》Chris Burges
Ranknet的经典之作。 <br>
* 采用梯度下降方法。
* 本质上是神经网络的一种实现。

* RankProp也是一种神经网络排序模型，它交替使用MSE regression和对当前排序结果的调整。它的输出是对期待排序的结果，而非具体的量化值。它的优点是作用于单独个体，而不是pair，但是我们不知道它在何种情况下收敛。

* PRank将特征向量转换为实数，学习时每次使用一个实例。
PRank是一种pair-based算法，需要在O(m^2)pairs而不是O(m)。
* RankBoost也是基于pairs来训练的，boosting也可以被视作一种梯度下降。

--------------
###《Learning to Rank for Information Retrieval》Tie-Yan Liu
inside search engine:<br>
![inside search engine](/images/jiqixuexi/LTR_ISE.png)<br>

**标注**：<br>
二分:相关/不相关<br>
多级：Perfect > Excellent > Good > Fair > Bad<br>
pairwise:文本对之间的判断。<br>

**评估**：<br>
**MRR** (Mean Reciprocal Rank)(即位置倒数的平均值):对于一个query，排序后第一个相关doc的位置为r[i]，则MRR为所有queries的1/r[i]的平均值。<br>

**WTA**(Winner Take All):对一个query的rank，若是第一个doc是相关的，那么打分为1，否则为0。WTA为所有queries的平均值。<br>

**P@n**(Precision at position n)：P@n={relevant documents in top results}/n(即rank结果中的前n项，相关doc的数目)<br>

**AP**(Average precision):(即把P@n进行累加后求平均。在这个过程中靠前的位置会累加多次，无形中体现了位置高的权重也高的思想。)<br>
假如排序如下：1,0,1,0,1(1代表相关)，则AP = 1/3*(1/1+2/3+3/5) = 0.76<br>

**MAP**(Mean Average Precision):就是把所有queries的AP求平均。<br>

可见上面的参数（MRR,WTA,P@n,AP,MAP）都是针对相关与否来做评判的。<br>

下面介绍的是跟多分类(假设是0--4档标注，值越大代表越好)标注相关的指标：<br>

**DCG**(Discounted Cumulative Gain):DVG=Σ(2^r(j)-1)/log(1+j)。(即各位置得分的和：label得分越高，url得分越高，排名越靠前，贡献系数越大)<br>

**IDCG**：理想化的DCG排序后的分值。<br>

**NDCG**:实际排序DCG/理想排序DCG。（即利用IDCG将DCG进行归一化）<br>
举例：对某个query的doc实际排序如下：2,0,1,2（理想的应该为2,2,1,0）那么：<br>
DCG = (2^2-1)/ log2 (1+1) + (2^0-1)/ log2 (2+1)+ (2^1-1)/ log2 (3+1) + (2^2-1)/ log2 (4+1)  = 4.79<br>
IDCG = (2^2-1)/ log2 (1+1) + (2^2-1)/ log2 (2+1)+ (2^1-1)/ log2 (3+1) + (2^0-1)/ log2 (4+1)  = 5.38<br>
NDCG = 4.79/5.38 = 0.89 <br>

**PAIR**：正序pair数量/逆序pair数量。<br>

传统IR模型：基于相关性模型（Boolean model/Vector space model等），基于概率模型（BM25 model等），基于超链接模型（HITS，PageRank等）<br>

使用ML的方式可以自动对参数进行调整，组合复杂信息，避免过拟合等。<br>
LTR就是将rank转换成传统的ML问题:<br>
Regression (assume labels to be scores)<br>
Classification (assume labels to have no orders)<br>
Pairwise classification (assume document pairs to be
independent of each other)<br>

在LTR中，相对之间的前后关系是重要的，不必在意具体的分类或是得分。并且越靠前的位置，权重越大。<br>
Categorization of LTR Methods:<br>
![Categorization of LTR Methods](/images/jiqixuexi/LTR_cateOfLTR.png)<br>
我们常用的就处于pair&classification
or regression 中，其中RankSVM和GBRank为最常用。<br>

**Pointwise Approach**:针对单一doc做分类或是回归。如Discriminative Model， McRank。<br>
**Discriminative Model for IR**:把相关的doc判定为正类，不相关的判定为了负类。可以使用svm或是MaxEnt(最大熵模型)来进行区分学习。<br>
**McRank**:使用gradient boosting来进行多分类。<br>

**Pairwise Approach**:不再像point的方式那样去预测绝对得分值，而是作用在文档对的基础上进行分类。<br>
**RankNet**:使用神经网络作为模型，使用梯度下降算法来优化交叉熵。(后续会详细地写)<br>
**RankSVM**:基本思路就是在pair上利用SVM进行二分类。
它利用了在rank中不必进行具体分值计算而是更关注相对前后位置的特性，这样两两文档之间的特征向量差就可以作为input。<br>
例如：d1,d2,d3它们的特征向量记为v1,v2,v3,并且d1好于d2好于d3。
那么v1-v2, v1-v3, v2-v3就可以作为为正样本,而v2-v1, v3-v1, v3-v2为负样本。<br>
然后利用SVM结构风险最小化思想来训练一个二分类器。
训练好之后，就可以来对比文档文档之间的好坏了。<br>
pairwise的缺点：没有体现出来位置的权重信息。<br>
















