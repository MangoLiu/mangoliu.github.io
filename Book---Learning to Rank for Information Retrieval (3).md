##Book---Learning to Rank for Information Retrieval (3)
Tie-Yan Liu  Microsoft Research Asia<br>
### The Pairwise Approach
#### 3.1 Overview
------------------
Pairwise Approach 相关的各种算法的区别主要在于loss function是不同的。<br>
pairwise不再关注单一doc的想相关性分值，而是关注于两个doc之间的相互序。此时ranking通常就转变为对doc pair的分类，例如判断pair中那个doc更好一些。<br>
学习的目标就是使分错类的pair数目最小。<br>


#### 3.2 Example Algorithms
------------------
Pairwise的方式就是在pair上判断，所以假设函数是h(Xu,Xv)表示判断两个文档Xu好于Xv的程度，取值在[-1,+1]之间，真实的好坏关系Yu,v={-1，+1}表示好或者坏。那么它的loss function可以定义如下：<br>
![lty_3](/images/liutieyan/lty_3.png)<br>
图像如下：<br>
![lty_31](/images/liutieyan/lty_31.png)<br>

注意：h函数仅仅是给出了一个pair的预测关系结果，并没有给出我们真正想要的list形式。所以需要一个额外的步骤来将任意pair的结果来处理成list形式。但通常pair的结果不会是完全正确，此时我们的问题就变成了在这种情况下什么样的order是最优的（即包含最少的错序pair）问题。这是一个NP问题，需要引入贪心策略。
[Learning to order things]()