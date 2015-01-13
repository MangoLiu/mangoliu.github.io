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
小流量：用户行为评估,前三点击率，首位点击率等<br>
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

2015-01-04问题记录：
```
DX的功能是什么？我们的点击模型是在DX模块中做的？
DX选取摘要和飘红处理，调用DC并使用基础相关性特征对TOP300计算得分。

为什么在训练AC和DX的时候会有不同？
{
	query：AC是线上随机选取；DX是偏重冷门与长Query。
	url:AC是baidu top30 + Google top10；而DX是baidu top20 + Google top10
}

在特征转化时：目前LTR特征中，按其转化类型分为五类，分别为：Linear、Bucket、TwoFeaturesLinear、 TwoFeaturesBucketLinear、Expression
这五类的主要应用情景？


AC要排两次序，而且两次的是不同的。
这两次是怎样在ltrtools中体现出来的？两次都是使用train.py，只是在cfg文件中配置的不同？

为什么不把飘红处理扔给更上层的UI模块来做？

UI和DA之间交流什么信息？
RS和EC？这是什么？

LTR评估模块不会影响特征转换吧？
（应该还是会的，假设模型不变，提取的特征不变，只是在map时有所改变。）

在LTR的模型训练中，我们现在有哪些模型可以选择？
```

siterank是站点等级。

=====2015-01-05日常学习记录-BEGIN=====：

ltrtools中的base_data可以理解成旧的数据，之前的数据。

参照ac_flow.cfg中的flow_all = update_feature, data_stat, ranktransform, mergeOptimizedObjectives, filtquery, filturl, train_ac, rankmodelranksvmmodel, testeval, wei_stat, staturl, addscore, analyse, showcase, cvtool, cvanalyse, cvshowcase, run_diff,  gen_report


**update_feature.py**:
在ac_flow.cfg中配置如下(部分)：
```
DATA_SET = data/ac/ac.final.data.dx_sample.1
BASE_DATA_SET = data/ac/ac.final.data.dx_sample.0
...
tool_name = update_feature
tool_addr = ./data_preprocess/update_feature.py
config_file= ./conf/data_preprocess/update_feature.cfg
new_data = %(DATA_SET)s
base_data = %(BASE_DATA_SET)s
out_data_update = data/ac/ac.final.data.dx
out_data_base = data/ac/ac.final.data.dx
delete_lost = true 

```
示例：更改DATA_SET，让它比BASE_DATA_SET，多了一列--url长度列（TEST_URL_LEN）。我们看看url长度模型的影响。<br>
它将更新后的结果保存在ac.final.data.dx中

我们需要修改config_file指示的配置文件：./conf/data_preprocess/update_feature.cfg，添加需要更新的项：<br>
```
TEST_URL_LEN = 0
```
这里的0代表默认值。<br>
在update_feature.py文件中，会将需要更改的项保存在feat_change_list中。<br>
```python
feat_change_list = config.items('update_feature')[1:] 
```
然后就可以看到ac.final.data.dx中多了一列TEST_URL_LEN。<br>

**data_stat**：<br>
```
TRANS_FILE = conf/ac/rerank.trans.lowquality_business_strike
OUTPUT_DIR = data/output/
...
tool_name = data_stat
tool_addr = data_analysis/data_stat.py
trans_file = %(TRANS_FILE)s 
feat_data = data/ac/ac.final.data.dx
result = %(OUTPUT_DIR)sdata_stat.result 
```
可见这里处理的就是我们上面刚刚更新好的ac.final.data.dx文件。<br>


=====2015-01-05日常学习记录-END=====

一些特征示例：
m:Label	m:Query	m:Urlno	F_UNI_WEI	F_BASIC_WEI	C_BASIC_WEI_SQR	F_LINK_WEI	F_LINK_WEI_MUL_BASIC	F_SCORE_LEVEL	CU1_IF_WENDA_DEMOTE	CU2_WENDA_DEMOTE	CU3_WENDA_DEMOTE_MUL_AUTHORITY	CU3_WENDA_DEMOTE_MUL_PAGERANK	CU3_WENDA_DEMOTE_MUL_LINK	CU1_IF_GENERAL_DEMOTE	CU2_GENERAL_DEMOTE	CU3_GENERAL_DEMOTE_MUL_AUTHOR	F_XUQIU_MATCH	CU1_VIDEO_XUQIU_PRI_PROMOTE	CU1_VIDEO_XUQIU_PAR_PROMOTE	CU1_OTHER_XUQIU_PRI_PROMOTE	CU1_OTHER_XUQIU_PAR_PROMOTE	FU1_BELIEF_WEIGHT	FU1_BELIEF_WEIGHT_IS_PROMOTE	FU1_BELIEF_WEIGHT_IS_SPAM	FU1_BELIEF_WEIGHT_IS_SPAM_MUL_LINK	FU1_BELIEF_WEIGHT_IS_SPAM_MUL_PAGERANK	FU1_PAGERANK_LOG_SOBAR_PV	FU1_BAIKE_REQ_PUNISH	F_BAIKE_NEWQUALITY	CU_ANTISPAM_MUL_AUTHOR	CU_ANTISPAM_MUL_LINK	CU_ANTISPAM_MUL_PAGERANK	CU_ANTISPAM_MUL_WENDA_MUL_AUTHOR	F_IS_SPAM_ZHUANTI	CU2_ZHUANTI_MUL_BELIEF	F_QUANXIAN	CU1_QUANXIAN	CU1_ENG_HIGH_AUTHORITY	CU1_ENG_MID_AUTHORITY	CU1_ENG_LOW_AUTHORITY	CU1_OTHER_HIGH_AUTHORITY	CU1_OTHER_LOW_AUTHORITY	CU1_SCORE_LEVEL	CU1_SCORE_LEVEL0	CU1_SCORE_LEVEL1	CU1_SCORE_LEVEL2	CU1_SCORE_LEVEL3	FA_SITE_TITLE	FA_SITE_ANCHOR	FA_SITE_CONTENT	FA_SITE_TERMCOUNT	FA_BELIEF_WEIGHT_SITE	CA_SEX_QUERY_SEX_PAGE	CA_NOSEX_QUERY_NOSEX_PAGE	CA_MUSIC_QUERY_MUSIC_PAGE	CA_NOMUSIC_QUERY_NOMUSIC_PAGE	F_WENWEN_RESULT	CA_WENWEN_IS_ANSWER	CA_WENWEN_IS_ANSWER_TITLE	F_ZHIDAO_RESULT	CA_ZHIDAO_IS_ANSWER	CA_ZHIDAO_IS_ANSWER_TITLE	C_NOVEL_PAGE_QUERY_EBOOK	C_SUBJECT_PAGE_FROM_TITLE	C_BUSINESS_PAGE_PRODUCT_QUERY	C_SOFT_PAGE_SOFT_QUERY	C_ZHAOPIN_PAGE_HIRE_QUERY	C_ZHAOPIN_PAGE_HIRE_QUERY_MUL_BASIC	C_TIME_SENSETIVE_PAGE_LOG_TIME	CA_IS_TIME_SESENTIVE_PAGE	F_QUERY_SYNC	F_QUERY_SPLIT	F_QUERY_FUNNEL_AT	F_QUERY_FUNNEL_CT	F_CLICK_INCREASE	F_OMIT_RES_EX	F_PROMOTE_TYPE	F_IS_ENG	F_QUERY_GUANLIAN	F_FUZZY_OFFSET	F_GENTIME_NEWS	F_CLICK_WEIGHT	F_CLICK_SATISFY	F_CLICK_NEED	F_CLICK_BELIEF	F_MLT_MATCH_BURST	F_OFFSET_LEVEL	F_REQUEST_PROMOTE_TYPE	F_CLICK_TIME_INCREASE	F_NOT_PAGE_ADJUST	F_KEYWORD_LAST_PAGE	F_DEADLINK_SECOND_PAGE	F_DEADLINK_LAST_PAGE	F_FROM_CONTENT	F_FROM_TITLE	F_FROM_ANCHOR	F_IS_ANTISPAM_PUNISH	F_PAGERANK	F_PT_WEI	F_PAGETYPE	F_IS_OTHER	F_BELIEF	F_BLOGID_WEI	F_DIR_AUTH	F_SOBAR	F_IS_ZHUANTI	F_VALIDITY_WEI	F_IS_SITE_RESULT	F_IS_ANSWER	F_QUALITY	F_NEWQUALITY	F_NEWQUALITY_MAP	F_SITE_DIR_URL	F_IS_BAIKE_RESULT	F_IS_BAIKE_PAGE	F_IS_ZHIDAO_PAGE	F_IS_WENKU_PAGE	F_IS_TIEBA_PAGE	F_IS_HI_PAGE	F_IS_WENWEN_PAGE	F_IS_ZIYOU_PAGE	F_DICT_BLOG_SITE	F_DICT_AUTHOR_SPAM	F_DICT_AUTHOR_PROMOTE	F_DICT_QUALITY_PROMOTE	F_DICT_ZHUANTI_SPAM	F_DICT_TRUST_SITE	F_DICT_VIDEO_SITE	F_DICT_SOBAR_PV	F_DICT_WENDA_PV	F_DICT_GENERAL_PV	F_SEX_PARA	F_POL_PARA	F_IS_DAILY_DNEWS	F_WEB20_PAGE	F_IS_TRUST_TIME	F_TIME_YEAR	F_TIME_MONTH	F_TIME_DAY	F_TIME_HOUR	F_TIME_MIN	F_TIME_SEC	F_ONLY_CONT_PAGE	F_NEWS_PAGE	F_BLOG_PAGE	F_BBS_PAGE	F_NOVEL_PAGE	F_SUBJECT_PAGE	F_CONTACT_PAGE	F_BUSINESS_PAGE	F_CGI_PAGE	F_KNO_PAGE	F_VIDEO_PAGE	F_BLOG_INDEX_PAGE	F_SOFTWARE_PAGE	F_INDEX_PAGE	F_DICT_GTIME_TRUST_SITE	F_DICT_ZHAOPIN_SITE	F_ISNOT_TIME_SESENTIVE_PAGE	F_IS_TIME_SESENTIVE_PAGE	F_MIS_WENDA_FLAG	F_MIS_GENERAL_FLAG	F_SITE_LAST_PAGE	F_SITE_SECOND_PAGE	C_TYPE_PIC_MAJOR_WEI	C_TYPE_PIC_MINOR_WEI	C_TYPE_MUSIC_MAJOR_WEI	C_TYPE_MUSIC_MINOR_WEI	C_TYPE_SOFT_MAJOR_WEI	C_TYPE_SOFT_MINOR_WEI	C_TYPE_MV_MAJOR_WEI	C_TYPE_MV_MINOR_WEI	C_TYPE_GAME_MAJOR_WEI	C_TYPE_GAME_MINOR_WEI	C_TYPE_EBOOK_MAJOR_WEI	C_TYPE_EBOOK_MINOR_WEI	C_TYPE_VIDEO_MAJOR_WEI	C_TYPE_VIDEO_MINOR_WEI	C_TYPE_PRODUCT_MAJOR_WEI	C_TYPE_PRODUCT_MINOR_WEI	C_TYPE_DRIVE_MAJOR_WEI	C_TYPE_DRIVE_MINOR_WEI	C_TYPE_DOC_MAJOR_WEI	C_TYPE_DOC_MINOR_WEI	C_TYPE_HIRE_MAJOR_WEI	C_TYPE_HIRE_MINOR_WEI	C_TYPE_MAJOR_WEI	C_TYPE_MINOR_WEI	C_PAGE_VIDEO_MAJOR	C_PAGE_VIDEO_MINOR	C_PAGE_SOFT_MAJOR	C_PAGE_SOFT_MINOR	C_SITE_VIDEO_MAJOR	C_SITE_VIDEO_MINOR	C_SITE_HIRE_MAJOR	C_SITE_HIRE_MINOR	C_SEX_QUERY_SEX_PAGE	C_RESOURCE_QUERY_VALIDITY_WEI	C_WENDA_NO_ANSWER	C_MAX_BLOGID_BELIEF	FU_CLICK	C_XUQIU_BAIKE	F_SOBAR_RANK	F_AUTHOR_SITE_BELIEF	F_AUTHOR_SITE_BELIEF_MAP	FU_AUTHOR_PR_WEI	FU_SITE_PR_WEI	FU_URL_PR_WEI	F_FIELD_MATCH_WEI	F_FIELD_TERM_COVERAGE	F_FIELD_SITE_HUB_WEI	F_FIELD_SITE_PR_WEI	F_FIELD_T_SCORE	F_FIELD_E_VALUE	F_FIELD_DANEEDS_MATCH	F_FIELDMATCH_QTAG_SPTAG_SQRT	F_FIELDMATCH_QTAGBIG_SPTAGBIG_SQRT	IS_NAV	F_IS_FROM_DX	F_QUERY_ANCHOR_MATCH_RATE	F_PR_ANCHOR_NOT_MATCH_WEI	F_SORTED_INFO_SITE_TYPE	F_EDITOR_LEVEL	F_FRESH_PAGERANK	F_BAD_ZHIDAO_PAGE	F_OSP_WEIGHT	F_OSP_COSIN	F_OSP_DOT	F_OSP_HIT_NUM	F_OSP_QUERY_TERM_NUM	F_OSP_HIT_RATIO	F_OSP_IS_OFFICIAL_URL	F_CM_DOC_LASTCLICK_RATE	F_CM_DOC_NAVCLICK_RATE	F_CM_DOC_CTR	F_CM_DOC_SKIP_PROB	F_CM_QD_LASTCLICK_RATE_OVERQUERY	F_CM_QD_LASTCLICK_CTR_NORM	F_CM_QD_LASTCLICK_RATE	F_CM_QD_NAVCLICK_RATE_OVERQUERY	F_CM_QD_NAVCLICK_CTR_NORM	F_CM_QD_NAVCLICK_RATE	F_CM_QD_CTR	F_CM_QD_SKIP	F_CM_QD_BOTHSIDE	F_CM_QD_NEXT_CLICK	F_CM_QD_PREV_NEXT_CLICK	F_CM_QD_SKIP_OVERQUERY	F_CM_QD_BOTHSIDE_OVERQUERY	F_CM_SATISFY_COUNT	F_CM_CLICK_COUNT	F_CM_EXAM_COUNT	F_CM_LTR_CLICK_MODEL	O_TIME_REQUEST_TYPE	O_TIME_REQUEST_EXT_VALUE	O_TIME_EXPECTATION	O_NEWS_EXPECTATION	O_IS_NEWS_QUERY	O_INCREASE_ADJUST_GENTIME	O_COULD_ADJUST	O_WEIGHT_0	O_WEIGHT_1	O_WEIGHT_2	O_WEIGHT_3	O_WEIGHT_4	O_WEIGHT_5	O_WEIGHT_6	O_WEIGHT_7	O_WEIGHT_8	O_WEIGHT_9	O_WEIGHT_10	O_WEIGHT_11	O_WEIGHT_12	O_WEIGHT_13	O_WEIGHT_14	O_WEIGHT_15	O_WEIGHT_16	O_WEIGHT_17	O_WEIGHT_18	O_WEIGHT_19	O_BASIC_WEIGHT_0	O_BASIC_WEIGHT_1	O_BASIC_WEIGHT_2	O_BASIC_WEIGHT_3	O_BASIC_WEIGHT_4	O_BASIC_WEIGHT_5	O_BASIC_WEIGHT_6	O_BASIC_WEIGHT_7	O_BASIC_WEIGHT_8	O_BASIC_WEIGHT_9	O_BASIC_WEIGHT_10	O_BASIC_WEIGHT_11	O_BASIC_WEIGHT_12	O_BASIC_WEIGHT_13	O_BASIC_WEIGHT_14	O_BASIC_WEIGHT_15	O_BASIC_WEIGHT_16	O_BASIC_WEIGHT_17	O_BASIC_WEIGHT_18	O_BASIC_WEIGHT_19	O_RESULT_NUM	O_EP_RET_ORING	O_EP_CTRL_DAY_ORIG	O_TYPE_PIC	O_TYPE_MUSIC	O_TYPE_SOFT	O_TYPE_MV	O_TYPE_GAME	O_TYPE_EBOOK	O_TYPE_VIDEO	O_TYPE_PRODUCT	O_TYPE_DRIVE	O_TYPE_DOC	O_TYPE_HIRE	O_KEEP_MIS_PUNISH	O_DO_CLICK_ADJUST	O_DO_CLICK_ADJUST_TIME	O_MAX_CLICK_WEIGHT	O_IS_SEXY_QUERY	O_AT_TERM_COUNT	O_CT_TERM_COUNT	O_AT_BASIC_TERM_COUNT	O_CT_BASIC_TERM_COUNT	O_QUERY_SEARCH	O_QUERY_SEARCH_TYPE	O_QUERY_CLICK	O_QUERY_SATISFY	O_OSP_IS_OFFICIAL_QUERY	F_LOWQUALITY_BUSINESS_SITE	TEST_URL_LEN	m:Url


基础相关性特
url特征(from Extractor) 
url字符串 
原始词频（TF） 
term基础权值：网页中的重要度[0-8]，在anchor中的重要度[1-256] 
语义特征：PLSA向量

query特征(from DA) query分析那边 
q_imp：term在query中动态重要性 
attr_type：属性词类型 
attr_level：属性词 需求强度 

query-url联合特征 (from BS)：主题匹配、offset、省略 
covered_title_ratio：query占网页主题比例 
hit_offset_ratio：query在页面命中term的紧密度信息。
term_reduce_ratio：惩罚 


性能指标评估：对每个训练好的模型，都可以计算其在测试集合上的性能指标，并可与其他模型指标进行对比。
评价标准使用Learning to Rank领域的常用指标，如NDCG,ERR,HERR等。

Case收益分析：对新模型的good/bad pair,good/bad case进行分析，并与基线模型的pair与case进行对比与变化分析。
评价指标：Diff面，Diff Query的收益。

特征影响评估：对特征在模型下的贡献及其变化做出评估。
纵向对比：特征权重变化评估：同一模型中，不同特征权重间的对比，主要考察特征权重间的大小关系是否合理。
横向对比：与基线的模型权重进行对比，主要考察同一维特征权重是否发生了较大变化，是否符合预期，例如basic_wei权重大幅下降是否正常。

前三相关性评估：新模型与基线模型对前三条检索结果的相关性进行对比分析，给出比较评分。

用户行为评估：在真是用户参与的情况下，根据用户的真实行为，对模型性能进行评估。
评价指标：前三点击率，首位点击率。

hi，各位同学：
今天任务：
1 标注query时效性，以及粒度。
2 新人接手串讲。

在串讲时提及到的不足和问题：
1 各项评价指标项中，哪个最为敏感？
2 pointwise Vs. pairwise
3 热门：冷门：长查询词=2:5:3的讨论
4 AC:30baidu_url+10google_url中Google的目前已经去掉。
  为什么是30？而不是别的？
5 update_feat是否更新了label？
6 高低频词筛选后，后面是怎么作用的？

以现在的时间点看query的时效性，并为其标注时间粒度。
打分的准则是看若是给出上一个粒度信息，用户的可接受程度。
举例：查看当日天气，那么粒度为天，若是给出昨天的，那么完全不可以接受，则打4分；
若是查看QQ绿钻年费，粒度为年，因为不经常变化。给出去年的年费也是有参考价值的，用户可以接受，打1分。

DAY:33/12 = 2.75
MONTH: 79/57 = 1.39
YEAR: 227/122 = 1.86
WEEK:22/10 = 2.2 

可以看出，DAY级的打分和粒度有之前的预期那样。
YEAR级别高于Month级别，主要是和伍艺讨论之后，把一些政策性，规章性的打分提高了一下。

DAY级别多出现于天气，贵金属，本地生活服务。
MONTH级别多出现于商品和明星类。
WEEK级别多出现于视频周更类。

在具体打分的时候，有很多有待商榷的地方，没有统一的打分规范，以上数据仅供参考。
