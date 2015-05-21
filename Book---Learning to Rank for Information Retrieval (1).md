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

**Vector Space model (VSM)**将doc和query都表示成欧几里得空间下的向量形式，这样通过两个向量的内积即可计算出它们之间的相似度。<br>







