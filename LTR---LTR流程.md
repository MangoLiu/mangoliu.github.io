#LTR流程
AC 主要处理querylist和doclist。
querylist是DA处理后给的。
doclist是BC根据一个query后给的。
每个query都会对应一个doclist，AC需要对这些list进行综合排序。
AC要做两次排序，一次是粗排，第二次是添加了DX特征的细排。

文本相关性是AC训练中最重要的特征。

click model是使用ranksvm通过以一些搜索，点击行为作为特征，来对url排序。

时效性是AC第二次排序时考虑的。

系统会根据用户的请求来识别他的需求（可能在不同的语言，不同的环境中），以此自动的来在众多策略中选取合适的策略版本。

统一调权策略是那些具有普适性的策略。权威性高的页面，肯定是要增加权值的；质量低的页面，肯定是要降的。

类聚策略：为了降低重复对用户的影响，控制最终结果中来自同一站点或相同内容的Doc的展现数量。

Spam惩罚: Spam结果不能出现在最终结果的Top 3中。

同义词风险控制: 同义词变换的结果，第一页最多只有五条, 多余部分会放到第二页。

AC的检索不光对外，还有对内的产品检索，例如文库，知道，贴吧等。

基础相关性，即网页与query的匹配程度是由DX计算出来的。

这个query的检索次数也是一维特征，它是有词典服务器记录的。用这个来区分是热门词或是冷门词。

是不是百度知道，是不是百度百科，是不是百度文库 也是一个维度。

AC简化流程如下：
* 队列检索召回
* 队列挖掘召回
* 第一次统一调权 做队列内排序
* 从DX特征服务器获取精细计算特征
* 在线统计
* 第二次统一调权: 利用精细特征， 重新做队列内排序
* 队列之间组合, 类聚 和 Punish

策略是否执行。策略开启需要在配置文件(strategy.conf)中增加相应的配置项并设置打开。

队列策略分成两种：一是召回型（Recall），二是挖掘型（Mining）。召回型通过query变换的方式获取bs结果得到队列，挖掘型从已经召回的队列中挖掘结果得到新的队列。


AC是基于GBrank的。

每个BS给它对应的BC top120条url<br>
每个BC给它对应的AC top760条url<br>
每个AC再首次排序后，给DX top300条url<br>
然后再对附有DX信息的300条url 进行 rerank。<br>

特征分为三类：<br>
1 页面自身特征<br>
2 页面和query之间的基础相关性特征<br>
3 用户点击行为特征。<br>

1 页面因子特征<br>
	score_level多term映射分层<br>
	Link_wei：链接权威性<br>
	Belief_wei:站点权威性<br>
	Sobar：获取热门度分档<br>
	newQuality：内容丰富度<br>
资源需求特征<br>
一些修正特征 <br>
	pdict_blog_site(blogidweight) <br>
	pdict_author_spam <br>
	pdict_author_promote <br>
其他独立特征 <br>
	是否英文页is_eng<br>
	是否繁体is_other<br>
	是否经过antispam惩罚<br>
	强泛时效性gentime_news<br>

2 基础相关性特征：<br>
url特征(from Extractor) ：<br>
	url字符串 <br>
	原始词频（TF） <br>
	term基础权值：网页中的重要度[0-8]，在anchor中的重要度[1-256] <br>
	语义特征：PLSA向量<br>
query特征(from DA) query分析那边 ：<br>
	q_imp：term在query中动态重要性 <br>
	attr_type：属性词类型 <br>
	attr_level：属性词 需求强度 <br>
query-url联合特征 (from BS)：<br>
	主题匹配、offset、省略 <br>
	covered_title_ratio：query占网页主题比例 <br>
	hit_offset_ratio：query在页面命中term的紧密度信息。<br>
	term_reduce_ratio：惩罚 <br>

3 点击特征(query <---> doc)<br>
在点击关系中，当查询一个query Q, 点击document D时，可视为Q对D的一次推荐，类似于文本分析中的anchor, Query Anchor即是将Q视作D的一个anchor.<br>
对于一个doc D，出现在不同query的搜索结果中，记这些query为{q1, q2, …, qn}，对每一个（D, qi）可以根据他们的点击关系计算一个socre, 这个score可以是任何反应D和qi之间相关性的分值。<br>


模型分为三类：<br>
pointwise<br>
pairwise(我们现在用的主要是pairwise)<br>
listwise<br>

评估分为三类：<br>
统计学指标：DCG,NDCG,ERR,HERR<br>
小流量<br>
全流量<br>

样本的标准分为两类：<br>
PM人工标注<br>
用户点击行为<br>

根据 AC 特征训练 RankSVM模型，得到第一次 AC 排序的 RankSVM 模型；<br>
根据 DX 特征训练 GBRank模型；<br>
调用 LTRTools 的 mergeacdx.py，将 AC 特征与 DX 特征融合在一起，利用URL UIRno匹配文件；<br>
调用 LTRTools 的 genacdata.py，使用 GBrank 模型的输出，更新 AC 特征中的 BasicWeight 特征；<br>
调用 LTRTools 的 runacdx.sh，训练二次 AC 排序GBRank模型。<br>
AC的第一次排序用ranksvm，第二次用gbrank<br>

RankSVM——线性模型<br>
优势：模型简单，解读性好，训练快速<br>
不足：特征不能自动交互，预测能力不足<br>
目前的解决办法：基于人工经验的特征变换和特征组合；特征进行分段/分片拆分

GBRank——基于决策树的非线性模型<br>
优势：非线性，特征自动交互，预测能力强<br>
不足：解读性较差；在标注样本稀疏的区域推广性差；过拟合的风险稍高<br>
目前的解决办法：有目的的添加样本；基于人工经验对loss <br>function和训练过程进行干预，样本采样，随即特征等方法解决过拟合；<br>

ltrtools中analyse功能：找到该模型下的good/bad & pair/case，分析起决定作用的特征参数。

阿拉丁是一个通用开放平台，它将接口开放给独特信息数据的拥有者，从而解决现有搜索引擎无法抓取和检索的暗网信息。
暗网就是爬虫无法抓取的网页，因为爬虫依赖页面中的链接关系发现新的页面，但是很多网站的内容是以数据库方式存储，典型的例子就是一些垂直领域网站。









