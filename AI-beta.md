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

TODO

--------------
###《Learning to Rank Tutorial》Tie-Yan Liu
LTR的入门引导。<br>
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

--------------

###《Optimizing Search Engines using Clickthrough Data》Thorsten Joachims
RankSVM的经典之作。<br>
使用人工标注数据来训练搜索引擎rank模型在现实中是困难而且昂贵的。我们使用用户的点击行为来训练模型代价很低。
我们把用户的点击行为记录在query-log之中。

三元组(q,r,c)表示一个query q，对应的ranking为r，在r上用户的点击行为记为c。这里r和c可以看做一个向量。可见，每一个query都对应着一个这样的三元组。

我们假设：用户会倾向于点击那些他更感兴趣，他认为更好的结果。

在实际中，用户的点击不光受相关性的影响，位置信息也会影响用户的点击。因为绝大多数用户仅仅关注Top N条结果。（之前yahoo!做过一个实验，把不好的url放在较高的位置也会有很大的点击量。）因此，不能将点击看做一个绝对的评判标准。

一个用户点击了第1,3,7条位置，虽然不能给出一个绝对意义上的估量。但是我们也可以从中得到一些有用的反馈信息。我们假设用户会从上而下浏览，在点击第3条之前，他浏览了第2条并且做了不去点击的决定。<br>
这样我们有理由认为第3条比第2条要好，第7条比第2,4,5,6条要好。但是我们无法比较第3和7条之间的优劣。


评价一个严格序列的指标：<br>
Kendall’s τ = (P-Q)/(P+Q),其中P为不一致pair的数目，Q为一致pair的数目。P+Q为所有pair的总和，即C(m,2),m为doc的数目。此时Kendall’s τ可以改写为：<br>
τ = 1-2Q/C(m,2)<br>
可以用τ的期望值来做为loss function。<br>

排序问题使用二分类(即分为“相关”/“不相关”)的弊端:<br>
1 训练集会非常的不平衡。因为大量的document都是不相关的。<br>
2 对相关的结果无法精细排序。<br>
3 无法使用上面所说的点击来进行优化。<br>

在样本集S上，有n个query，每个对应一个理想序列r*。我们的目标就是：1/n*sum{τ(r(q[i]),r*[i])}最大化。就是该样本集上所有query的平均τ值最大。

假设我们有个线性函数f(q,d)能来衡量document在这个query下的相关值。线性函数f的参数为一个向量w。<br>
那么在有n个query的样本集中，每个query对应的一系列document彼此之间均可进行相关值的大小的比较，这样可比较的pair共有K对。那么此时，我们目标就变为找到一个向量值w，来使满足不等式比较关系的pair数目尽可能多。<br>
但是直接求解上面的问题，这是一个NP问题。<br>

采用SVM的思想：因为我们有训练样本，每个样本其实是一个向量，令其为Φ。若d1好于d2，Φ1-Φ2的结果也是一个向量，这个向量为正样本，反之Φ2-Φ1这个结果向量就是一个负向量。如此任何两个样本作差，就得到了正负样本集。这样就可以应用SVM的方式进行分类，目标是使结构风险最小化。

由上面我们知道，数据的标注来源于click，那么我们就无法得知最理想的序列。那么我们就得需要以当前可以观察到的偏好序列r'来代替理想序列r*。



























