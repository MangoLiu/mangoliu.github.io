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
**Mean Reciprocal Rank (MRR)**(平均倒数排名)：








