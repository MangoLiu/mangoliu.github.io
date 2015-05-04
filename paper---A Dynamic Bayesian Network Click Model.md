#PAPER---A Dynamic Bayesian Network Click Model
Olivier Chapelle-Yahoo! Labs-Santa Clara, CA<br>
Ya Zhang-Yahoo! Labs-Santa Clara, CA <br>

##ABSTRACT
基于click log可以获得很多有用的信息，但相比于人工标注，它最大的难点在于position bias。即即便都相关，用户倾向于点击位置较高的的url。<br>

##INTRODUCTION
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

##MODELING PRESENTATION BIAS
###position models
position model的核心假设是一条点击意味着这条url被用户检查并且认为它具有相关性。同时中各个被检查的概率仅仅与位置相关。这样的话，给定一个url u和位置 p，我们就可以预测它的点击率（其中E表示检查，C表示点击）：<br>
![pos model](/images/paper_dbn_pos1.png)<br>








