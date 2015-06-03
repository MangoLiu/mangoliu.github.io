##Book---Learning to Rank for Information Retrieval (1)
Tie-Yan Liu  Microsoft Research Asia<br>

#### Overview
------------------
一般来说搜索引擎包含六大部分: crawler, parser,indexer, link analyzer, query processor, and ranker.<br>
![lty_1](/images/liutieyan/lty_1.png)<br>
crawler从web上抓取网页和文档；parser分析文档和超链接图；indexer对索引和数据进行建库；link analyzer跟据超链接图来分析网页的权重；query processor对query进行处理（类似NLP的工作，切词、改写、term重要性之类的）；ranker是central component，它将处理后的query和检索出的网页进行排序处理。<br>

ranking在信息检索应用中是一个很核心的问题。像是协同过过滤，问答系统，多媒体检索，文本摘要以及在线广告等均有涉及ranking。利用machine learning的方式处理ranking问题即为learning to rank，即LTR.<br>

#### Ranking in Information Retrieval
--------------------
传统的ranking模型可以分为relevance ranking models和importance
ranking models.<br>
relevance ranking就是从相关性角度计算出每一个doc和query之间的分值。早期的ranking model就是基于query-terms在doc中的出现次数来计算的。例如**Boolean model**就是预测doc相关与否，但它不能给出具体的度量值。<br>

**Vector Space model (VSM)**将doc和query都表示成欧几里得空间下的向量形式，这样通过两个向量的内积即可计算出它们之间的相似度。注意，VSM模型的一个隐含假设是term之间彼此独立。<br>

衡量指标IDF(t):<br>
![lty_11](/images/liutieyan/lty_11.png)<br>
其中N是这个term在整个文档集合中的总次数，n(t)是当前这个文档包含该term的次数。<br>


**Singular Value Decomposition (奇异值分解，SVD)**将原始特征空间线性的转换到潜在语义空间，在这个空间中来定义query和doc之间的相关性。<br>


**BM25**不是一个单独的model，而是一系列的ranking model，每一个model的不同的组成和参数。<br>
![lty_12](/images/liutieyan/lty_12.png)<br>
d表示doc，q表示query，q包含了M个term，TF(t,d)表示该term在该doc的频率，即该term数量/文档term总数量；LEN(d)表示文档的term数量，avdl表示相关文档集中平均文档长度，即term数量；k1和b是自由参数。<br>
注：BM25大体表述了，一个doc包含某个term相对数越多，并且该doc越短，这个doc和这个term就越相关。k1和b可以理解为和term相关的参数，因为在一个query的不同term，权重、属性等是不同的。<br>


有些model的rank依据是他们自身的重要性，例如**PageRank**，PageRank的计算公式如下：<br>
![lty_13](/images/liutieyan/lty_13.png)<br>
其中dv是有链接指向du的page，U(dv)是dv页面中的链接的数目。<br>
可以看出，一个page的value，取决于指向它的网页的权重，以及这个网页中的数量。它的含义是若是权重很大的网页指向了当前页，那么当前页的权重也可能很大，并且假设用户点击一个网页上的链接是随机均匀的。<br>


相关性评估，后续会有三种方式：<br>
1 Relevance degree（这也就是后续要提到的pointwise），对query和doc进行打分，打分可以有很多不同的形式，例如相关与否（1或0），或是相关的程度（Perfect, Excellent, Good, Fair, or Bad）等等。刻画query和doc间的相关性信息。<br>
2 Pairwise preference，人工去判断在某个query下，一个doc和另一个doc谁更加相关，用来描述在两个doc之间的相对关系。刻画的是doc和doc之间的相对性关系。<br>
3 Total order （这个也就是后面要提到的listwise），人工标注出一个query下所有doc的序列关系。<br>


上述提及的三种方式，第一种是在实际评估中最经常用到的。因为它人工成本较低，仅需要评估单个的doc和query间的关系。而第三种的人工成本非常的高。<br>
下面介绍几种评估的量化指标：<br>
**Mean Reciprocal Rank (MRR)**(平均倒数排名)：对于一个query，在rank排序后，第一个相关doc的位置为r，那么1/r本定义为MRR。<br>


**Mean Average Precision (MAP)**，要想定义MAP，首先得先了解Precision at position k(P@k)
![lty_14](/images/liutieyan/lty_14.png)<br>
即定义一个二分判断函数，判断doc是否相关，然后看前K个结果中有多少个相关doc。<br>
这样Average Precision (AP)的定义如下：<br>
![lty_15](/images/liutieyan/lty_15.png)<br>
这个AP衡量的是相关doc的平均precision值，并且有考虑位置因素，即越高位的权重越大些。因为在搜索引擎中，高位的权重要比低位的大，即同样是排错，但是高位排错的影响更大。

在test query的全集上的平均AP值被称为MAP.<br>

举个例子：<br>
![lty_16](/images/liutieyan/lty_16.png)<br>
P@1 = 1,P@2 = 1/2,P@3 = 2/3.<br>
AP = 1/2 * (1 * 1 + 1/2 * 0 + 2/3 * 1) = 5/6<br>


**Discounted Cumulative Gain (DCG) and  NDCG**:DCG是考虑了位置因素在其中，位置越高权重越大。IDCG是在最理想情况排序的DCG值，NDCG = DCG/IDCG。所以可以看出NDCG的值在0-1之间，并且值越大说明越接近理想情况。<br>
以之前在组里做分享的例子来说明：<br>
![lty_17](/images/liutieyan/lty_17.png)<br>


**Rank Correlation (RC)**:用来衡量model结果的ranked list 和一个给定的相关性list的指标。定义如下：<br>
![lty_18](/images/liutieyan/lty_18.png)<br>

上述的评估方式都是position based的，即对一个doc打分的扰动，当不足以影响整个序列顺序的时候，对评估指标是不影响的。<br>
并且这些评估方式也都是query level的，即对每个query独立的评估，然后再看在整个集合上的表现。<br>
---------------

机器学习的几个关键组成部分：<br>
1 input space ，以特征向量来表示的样本，特征是以某种方式提取出来的。<br>
2 output space ,对input处理后的展现形式，包括两种，一是task的最后形式，另一种是meachine learning 中间便于处理的形式。<br>
3 hypothesis space(假设空间)，它定义了一系列从input space 到output space的映射函数。<br>
4 loss funciton:ML的目的就是在training set上定义一个loss function来学习一个optimal hypothesis。loss function在ML中扮演了一个非常关键的角色。<br>
![lty_19](/images/liutieyan/lty_19.png)<br>

----------------
LTR的定义：<br>
广义定义：使用机器学习技术来解决ranking问题的都称为ltr方法。<br>
狭义定义：满足以下两点的ranking方法：<br>
1 Feature Based，样本以特征向量的形式体现。<br>
2 Discriminative Training，(有识别性的训练)a learning-to-rank method has its own input space, output space, hypothesis space, and loss function.Discriminative Training是一个基于训练集的自动学习过程。<br>

---------------
LTR框架<br>
![lty_110](/images/liutieyan/lty_110.png)<br>
ltr是有监督学习，因此是需要有标注的training set。很多机器学习都可以套在上述流程图中。不同的方法主要体现在不同的input space，不同的的output space，不同的假设空间，不同的损失函数。<br>

The Pointwise Approach：<br>
输入是一个单独的特征向量表示的数据，输出是该doc与query的相关性程度打分。loss function 可以根据打分函数的形式来进行选择。<br>
使用scoring function对doc进行打分，是query和doc的绝对打分。把一个query下的所有query全部进行打分，然后按照打分进行排序。pointwise没有考虑doc和之间的关系，也没有考虑到position因素的影响。<br>
对一个query下的各个doc xi的打分为ci，偏序关系如下：<br>
![lty_111](/images/liutieyan/lty_111.png)<br>
注：上图来源于网络。<br>

