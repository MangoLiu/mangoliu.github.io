##PAPER---A Dynamic Bayesian Network Click Model
Olivier Chapelle-Yahoo! Labs-Santa Clara, CA<br>
Ya Zhang-Yahoo! Labs-Santa Clara, CA <br>

####ABSTRACT
基于click log可以获得很多有用的信息，但相比于人工标注，它最大的难点在于position bias。即即便都相关，用户倾向于点击位置较高的的url。<br>

####1 INTRODUCTION
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

####2 MODELING PRESENTATION BIAS

position model的核心假设是一条点击意味着这条url被用户检查并且认为它具有相关性。同时中各个被检查的概率仅仅与位置相关。这样的话，给定一个url u和位置 p，我们就可以预测它的点击率（其中E表示检查，C表示点击）：<br>
![pos model](/images/paper_dbn_pos1.png)<br>
上面式子利用了下面如下的假设：<br>
1.若是没有检查则一定没有点击。<br>
2.若是url被检查了，那么点击的概率仅仅取决于相关性。<br>
3.url是否被检查的概率仅仅与位置pos有关。<br>

如上式子中我们可以把结果看作两个部分的乘积，αu和βp，其中αu可以看做是url与query的相关性表示，而βp可以看做是位置的影响。<br>
想想我们工作的目标，利用click log评估url的相关程度。这不正是αu所表示的么，我们变化一下等式，P(C=1|u,p)这个是通过clicklog统计出来的，而βp就是我们之前所言的不同位置的检查概率，这是已知的，因此αu就可以计算了。<br>

####COEC model(clicks over expected clicks)
βp我们可以简单的位置的点击率来估计，假设一个query有相关的N个session，第i个session中的点击与否为ci和位置的bias为βpi，此时，αu可以表示为：<br>
![pos model coec](/images/paper_dbn_coec.png)<br>

COEC model的问题在于对于β的估计是有偏倚的。如果搜素引擎是对结果随机排序的话，那么这个模型是有效的。但是通常相关性较好的结果通常排名较高，这样此时计算出来的CTR(即点击率)不仅仅包含了位置的偏倚信息，同时还包含了该位置相关性的信息。<br>

####Examination model
给定向量βp，对于αu的最大释然估计如下：<br>
![pos model max](/images/paper_dbn_max.png)<br>
下标i表示session i对应的变量。<br>

这个模型的缺点是αu可能会大于1。<br>

####Logistic model
![logistic.png](/images/paper_dbn_logistic.png)<br>

####Cascade Model
瀑布模型和上述的位置模型有些不同。它认为在结果页面中所有网页彼此是独立的，假设用户会从上而下完全浏览过后再去选择点击哪一个结果。<br>
假如用户点击第i个位置，表示用户检查了前i-1个结果，决定skip；并且用户认为这个第i个结果满足了他的需求，忽略后面的结果。
即在这个模型中仅有一次点击，并且是满意点击。这样，给出第i个位置的点击概率：<br>
![logistic.png](/images/paper_dbn_cascade.png)<br>

####3 DYNAMIC BAYESIAN NETWORK
DBN在点击日志中估计url的相关性时，把整个结果集当作一个整体，同时考虑各个结果之间的影响。<br>
![logistic.png](/images/paper_dbn_dbn.png)<br>
框里定义的是session级别的变量；框外定义的是query级别的变量。<br>
变量说明：<br>
Ei: 用户是否检查了这个url？<br>
Ai: 在感知相关性上，用户是否被这个url吸引？<br>
Si: 用户是否对着陆页上的内容满意？<br>

给出了下面几天很重要的假设：<br>
**Ai = 1,Ei = 1 <==> Ci = 1 **<br>
用户检查了并且感兴趣，那么就会点击。<br>
P(Ai = 1) = au <br>
用户检查时，感兴趣的概率仅仅与url本身有关。<br>
P(Si = 1|Ci = 1) = su<br>
用户点击一个结果，满意的概率就是su.<br>
Ci = 0 ==> Si = 0<br>
若是没有点击，那么该条结果的满意度一定为0。<br>
Si = 1 ==> Ei+1 = 0<br>
若是用户对一个结果满意，那么是不会去点击下一条的。<br>
P(Ei+1 = 1|Ei = 1, Si = 0) = γ <br>
若是用户检查了某条结果，但是不满意，那么它检查下一条结果的概率为γ，γ刻画了用户对搜索引擎结果的容忍程度，即不满意当前继续往下看的概率。<br>
Ei = 0 ==> Ei+1 = 0<br>
这是假定用户是自上而下浏览的，要是没有检查第i条，则是不会浏览i+1条的。<br>

这里的model有两个变量：au,su。前者刻画了感知相关性，后者刻画了点击后的满意程度。<br>
ru :=P(Si = 1|Ei = 1)
=P(Si = 1|Ci = 1)P(Ci = 1|Ei = 1)
=au*su<br>
可以看出这个模型在建模实际相关程度，而非感知相关程度。<br>

####Link with other models
examination model可以看作是上述model的特例，它的Ei是独立，仅与位置有关的model。<br>
cascade model也是上述model的特例，γ = 1 and su = 1。用户会一直检查，直到看到满意的结果，点击，并离开。<br>

skip-above pairs:若是用户点击没有点击i,而是点击了j，并且j>i。那么说明doc j好于doc i。<br>

#### Inference
注:这里涉及到很多模型比对的实验数据，不一一列出，有兴趣可以查询原paper。<br>
DBN可以看作是一个稍稍复杂一点的隐性马尔科夫模型（HMM）。<br>

DBN模型需要参数γ，通过实验DBN model在γ=0.9时MSE最小。我们可以直接利用这个结论，再后续的实验中直接使用0.9这个值。<br>

logistic model在position1 和position2之间有剧烈的抖动，原因是一些展示结果中当第一条非常好的时候，用户几乎不会检查第二条。这种情形多发生于导航寻址型query。<br>

在实际的rank中，我们更加关注的是预测点击率间的相对序，而非具体的绝对数值。我们经常采用NDCG来做为衡量指标。<br>

通常，DBN model好于cascade model和logisti model，但随着session的数据量的增大，差距逐渐减小。<br>

####CLICKS VS EDITORIAL JUDGMENTS
使用点击数据和使用人工标注数据可能会有些偏颇差异，差异点主要源自一下两点：<br>
1.点击强和相关性没有必然联系。<br>
2.点击反映的是感知相关性，而人工标注更加关注的是真实相关性。
举个例子：query =“adobe”，title=" www.adobe.com "看上去不错，但是用户可能更多点击的是adobe reader的链接。这个例子说明，有时人工标注并不一定能很好的进行判断，而通过click却可以知道用户的需求。


