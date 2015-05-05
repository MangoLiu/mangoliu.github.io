#PAPER---A Dynamic Bayesian Network Click Model
Olivier Chapelle-Yahoo! Labs-Santa Clara, CA<br>
Ya Zhang-Yahoo! Labs-Santa Clara, CA <br>

##ABSTRACT
基于click log可以获得很多有用的信息，但相比于人工标注，它最大的难点在于position bias。即即便都相关，用户倾向于点击位置较高的的url。<br>

##1 INTRODUCTION
人工标注的缺点：代价非常昂贵，并且label值可能会随着时间的变化而变化。<br>

click的多种用途：调节搜索引擎的参数，评估不同的rank方法，或是作为ranking的反馈信息。<br>

有两种类型的click model：position model和cascade model，即位置模型和瀑布模型。<br>
position model假设click与“相关性”和“检查”有关。每一个url都有一个确定的概率被检查，这个概率的衰减仅仅与rank有关（即与某个具体的query检索无关）。它给出了一个基于rank整体的各个pos的检查概率。例如，通过统计我们的得知在前十位的检查概率为：0.61,0.42,0.21,0.09....那么所有query的检索都是使用这个概率list。<br>

cascade model假设用户顺序的检查结果，一旦点击某个具有相关性的url，则停止其检索行为。<br>
但是这个模型不能解释用户离开以及多次点击的情形。<br>

本paper提出的动态贝叶斯网络模型（DBN）是基于用户浏览行为模型来构造的。<br>

DBN和cascade model的区别主要在两个方面：<br>
1. 由于一次click并不一定意味着是满意点击。我们试图去区分感知相关性和真实相关性。<br>
2. 我们没有去限制click的次数。<br>

我们的目标是预测出一个url与query的相关程度，这个结果我们用两种使用用途：<br>
1.作为rank方法中的一列特征。(liu：用于训练)<br>
2.在rank方法在学习过程中用作补充数据。(liu：用于评估)<br>

##2 MODELING PRESENTATION BIAS
###2.1 position models
position model的核心假设是一条点击意味着这条url被用户检查并且认为它具有相关性。同时中各个被检查的概率仅仅与位置相关。这样的话，给定一个url u和位置 p，我们就可以预测它的点击率（其中E表示检查，C表示点击）：<br>
![pos model](/images/paper_dbn_pos1.png)<br>
上面式子利用了下面如下的假设：<br>
1.若是没有检查则一定没有点击。<br>
2.若是url被检查了，那么点击的概率仅仅取决于相关性。<br>
3.url是否被检查的概率仅仅与位置pos有关。<br>

如上式子中我们可以把结果看作两个部分的乘积，αu和βp，其中αu可以看做是url与query的相关性表示，而βp可以看做是位置的影响。<br>
想想我们工作的目标，利用click log评估url的相关程度。这不正是αu所表示的么，我们变化一下等式，P(C=1|u,p)这个是通过clicklog统计出来的，而βp就是我们之前所言的不同位置的检查概率，这是已知的，因此αu就可以计算了。<br>

###2.1.1 COEC model(clicks over expected clicks)
βp我们可以简单的位置的点击率来估计，假设一个query有相关的N个session，第i个session中的点击与否为ci和位置的bias为βpi，此时，αu可以表示为：<br>
![pos model coec](/images/paper_dbn_coec.png)<br>

COEC model的问题在于对于β的估计是有偏倚的。如果搜素引擎是对结果随机排序的话，那么这个模型是有效的。但是通常相关性较好的结果通常排名较高，这样此时计算出来的CTR(即点击率)不仅仅包含了位置的偏倚信息，同时还包含了该位置相关性的信息。<br>

###Examination model
给定向量βp，对于αu的最大释然估计如下：<br>
![pos model max](/images/paper_dbn_max.png)<br>
下标i表示session i对应的变量。<br>

这个模型的缺点是αu可能会大于1。<br>

###Logistic model




