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

#### Learning a combination of ordering functions
------------

#### Experimental results for metasearch
------------


#### Related work
------------

#### Conclusions
------------



