##Paper---Learning to order things
Cohen,Schapire,Singer  AT&T shannon laboratory<br>

在LTR中，最多使用的就是pairwise，而在pair的比较中就是将比较两个doc who is better看作为一个分类方法。若是对一个拥有一个dataset中任意两个doc的比较关系，我们可以给出整个序列。但在实际情况下pair并非完全正确，即有部分错误的。那么此时如何找到最优list？（即在这个list中，逆序关系最少？）
这就是本文要介绍的。

#### INTRODUCTION
------------
如上所述，这个问题是一个NP问题，这篇paper会介绍一个简单的基于贪心的策略。<br>
我们令preference function 为Rf:<br>
![1](/paper/paper_ltot_1.png)<br>
Rf(u,v)=1，意味着u好于v，反之。<br>


#### ordering instances with a preference function
------------
为了衡量order，我们需要先定义一下衡量的标准。<br>
![2](/paper/paper_ltot_2.png)<br>
表示在total ordering 中正序的pair数目。<br>
注：我感觉任何NP问题都可以通过调优算法来解决，以一种可以接受的good结果来代替最优的best结果，使用的方法可以是爬山算法，模拟退火算法，遗传算法等。<br>

下面给出一个基于preferences的贪心方式：<br>
![3](/paper/paper_ltot_3.png)<br>
这个方法就是将docs看作是一个有联系的图，每个doc是一个节点，节点之间的连线就是彼此之间的PREF()的计算值。<br>

解释：<br>
INPUT当然是一个doc集合X，以及一个可以比较任何两个doc之间好坏的PREF函数。<br>
OUTPUT一个接近最优解的order。<br>
初始时，计算一个点的出度入度的和。（其实，这样做是因为有的情形下|PREF(u,v)| != |PREF(v,u)|。要是|PREF(u,v)| == |PREF(v,u)|，可以仅考虑PREF(v,u)）。选择值最大的，挑选出去，然后在剩余的基础上重新计算各个点的出入度值（其实就是加上原来它们和挑选出去node之间的值，也就是while语句块中的最后一句代码所示。）直至全部挑选出去，这个挑选序列就是最终的顺序。<br>
当|PREF(u,v)| == |PREF(v,u)|时，如下：<br>
![4](/paper/paper_ltot_4.png)<br>



#### Learning a good weight vector 
------------


#### Related work
------------

#### Conclusions
------------



