#音频广告推送模型---SVM以及libsvm的应用
--------------------------------
##SVM
本文需要有SVM的理论知识背景，若是对SVM不熟悉，请先参考：<br>
`《数据挖掘导论》` (人民邮电出版社)，相对简单入门。<br>
`《统计学习方法》` (清华大学出版社)，相对深入数学原理。<br>
`《机器学习实战》` (人民邮电出版社)，相对偏重应用(python编写)。<br>
三本书中都有涉及SVM的基本知识。而且讲的不错。<br>
再额外推荐下july写的[支持向量机通俗导论(理解SVM的三层境界)](http://blog.csdn.net/v_july_v/article/details/7624837)

##libSVM
本文主要讲[libsvm(百度百科)](http://baike.baidu.com/view/598089.htm?fr=aladdin)的学习和项目的中的实际应用。<br>

<strong>1 下载libsvm，引入java包。</strong><br>
libsvm主要有5个文件夹：
Java:主要是应用于java平台；<br>
Python:是用来参数优选的工具；<br>
svm-toy:一个可视化的工具，用来展示训练数据和分类界面；<br>
tools:主要包含四个python文件，用来数据集抽样(subset)，参数优选（grid），集成测试(easy), 数据检查(checkdata)；<br>
windows:包含libSVM四个exe程序包，里面还有个heart_scale，是一个样本文件，用来测试用的。<br>

<strong>2 在主程序中进行调用。</strong><br>
```java
String[] arg = {"train", "model" };//训练所需的参数
String[] parg = {"test", "model", "output"};//预测所需的参数
svm_train t = new svm_train();
svm_predict p = new svm_predict();

try {
	t.main(arg);
	p.main(parg);
} catch (Exception e) {
	e.printStackTrace();
}
```
<br>
其中，train包含了训练文本，然后将训练好的模型保存在model之中。<br>
然后，利用model，对包含测试文本的test进行预测，将结果保存在output之中。<br>



<strong>3 可以看到运行结果(举例)：</strong><br>
>  optimization finished, #iter = 4785 <br>
   nu = 0.8409951325040563 <br>
   obj = -3106.533687039116, rho = -0.7652400565491617<br>
   nSV = 3168, nBSV = 3058<br>
   Total nSV = 3168<br>
   Accuracy = 43.333333333333336% (26/60) (classification)<br>
-----------<br>
>
   其中，#iter为迭代次数，<br>
   nu 是你选择的核函数类型的参数，<br>
   obj为SVM文件转换为的二次规划求解得到的最小值，rho为判决函数的偏置项b，<br>
   nSV 为标准支持向量个数，nBSV为边界上的支持向量个数(a[i]=c)，<br>
   Total nSV为支持向量总个数（
   对于两类来说，因为只有一个分类模型Total nSV = nSV，但是对于多类，这个是各个分类模型的nSV之和）。<br>
<br>

在目录下，还可以看到产生了一个train.model文件，可以用记事本打开，记录了训练后的结果。
>
   svm_type c_svc         //所选择的svm类型，默认为c_svc<br>
   kernel_type rbf        //训练采用的核函数类型，此处为RBF核<br>
   gamma 0.125            //RBF核的参数γ<br>
   nr_class 2             //类别数，此处为两分类问题<br>
   total_sv 3168          //支持向量总个数<br>
   rho -0.76524           //判决函数的偏置项b<br>
   label 1 0              //原始文件中的类别标识<br>
   nr_sv 1613 1555        //每个类的支持向量机的个数<br>
   SV                     //以下为各个类的权系数及相应的支持向量<br>
   1.0 1:0.9166666666666666 2:0.4117647058823529 3:0.4818941504178273 4:0.0 5:0.0 6:0.020029920044170726 7:5.281652957486361E-4 8:3.5202955497043865E-4 <br>
   1.0 1:0.5833333333333334 2:0.08823529411764706 3:0.4596100278551532 4:0.0 5:0.0 6:0.9805126532149177 7:0.012915604324685265 8:0.020193845364303305 <br>
   ......<br>

<strong>4 svmtrain操作参数说明</strong><br>
以下引自网络
```
svmtrain主要实现对训练数据集的训练，并可以获得SVM模型。
用法： svmtrain [options] training_set_file [model_file]
其中，options为操作参数，可用的选项即表示的涵义如下所示:
-s 设置svm类型,默认0：
    0 –- C-SVC
    1 –- v-SVC(nu-SVC)
    2 –- one-class-SVM
    3 –- ε-SVR
    4 –- n-SVR

-t 设置核函数类型，默认值为2。(γ即为gamma)
    0 -- 线性核：u'*v
    1 -- 多项式核：(g*u'*v+ coef 0)^degree
    2 -- RBF核：exp(-γ*||u-v||^2)
    3 -- sigmoid核：tanh(γ*u'*v+ coef 0)

-d degree: 设置多项式核中degree的值，默认为3

-gγ: 设置核函数中γ的值，默认为1/k，k为特征（或者说是属性）数；
    -r coef 0:设置核函数中的coef 0，默认值为0；
    -c cost：设置C-SVC、ε-SVR、n-SVR中从惩罚系数C，默认值为1；
    -n v ：设置v-SVC、one-class-SVM 与v-SVR中参数n，默认值0.5；
    -p ε ：设置v-SVR的损失函数中的e ，默认值为0.1；
    -m cachesize：设置cache内存大小，以MB为单位，默认值为40；
    -e ε ：设置终止准则中的可容忍偏差，默认值为0.001；
    -h shrinking：是否使用启发式，可选值为0 或1，默认值为1；
    -b 概率估计：是否计算SVC或SVR的概率估计，可选值0 或1，默认0；
    -wi weight：对各类样本的惩罚系数C加权，默认值为1；
    -v n：n折交叉验证模式；

注意：上文中的wi，由于C是惩罚因子，可以针对正负样本使用不同的惩罚值。 
这个惩罚值是指 对要训练的分类器 发生误判的惩罚程度。
比如：正负样本比例为1:9. 我们需要加大惩罚力度。
结合Libsvm FAQ中所说的例子，“svm-train -s 0 -c 10 -w1 1 -w-1 5 data_file 。 
the penalty for class "-1" is larger. Note that this -w option is for C-SVC only”。
那此处对于我举的例子，命令应该是：svm-train -s 0 -c 10 -w1 9 -w-1 1 data_file 
相当于采用C-SVC模式，对+1误判惩罚为10*9=90, 对-1误判为10*1=10.

model_file：可选项，为要保存的结果文件，称为模型文件，以便在预测时使用。
默认情况下，只需要给函数提供一个样本文件名就可以了，但为了能保存结果，还是要提供一个结果文件名。
比如:test.model,则命令为：svmtrain test.txt test.model
```
<strong>5 svm类型</strong><br>
通过设置-s来，指定不同的svm类型那个。<br>
    0 –- C-SVC<br>
    1 –- v-SVC(nu-SVC)<br>
    2 –- one-class-SVM<br>
    3 –- ε-SVR<br>
    4 –- n-SVR<br>
前三个是分类的，后两个是回归的。<br>
c-svc和 nu-svc本质差不多,c-svc中c的范围是1到正无穷;<br>
nu-svc中nu的范围是0到1，还有nu是错分样本所占比例的上界，支持向量所占比列的下界。<br>
在libsvm中，不同的svm类型意味着不同的模型优化函数和不同的决策函数。<br>
![C-SVC](/images/C-SVC.png)
![V-SVC](/images/V-SVC.png)
![one-class SVM](/images/one-class SVM.png)
![epsilon-SVR](/images/epsilon-SVR.png)
![V-SVR](/images/V-SVR.png)<br><br>

<strong>6 核函数</strong><br>
核函数的目的就是在非线性可分的情况下，通过一个映射函数，将低维的输入空间映射到高维的特征空间。<br>
这样，低维的线性不可分问题就变成高维空间的线性可分问题。<br>
![map_kernel](/images/kernel.png)<br>
目前常用的核函数有如下4种：<br>
![4_kernels](/images/kernel_four.png)<br>
其中，RBF核函数是一个应用较为广泛的核函数，通过参数的选择，它可以使用于任意分布的样本。<br>
RBF含有两个参数误差惩罚参数C和高斯核参数γ。其中两者对错误率的影响如下：<br>
![C&R](/images/C&R.png)<br>
因此，对于一个基于RBF核函数的SVM，其性能是由参数(C,γ)决定。<br>
<br>
<strong>7 参数调优原理</strong><br>
由于选取不同的C和γ就会得到不同SVM，常用的参数寻优方式如下：<br>
<strong>7.1 双线性搜索法</strong><br>
其原理是利用不同的(C,γ)取值对应不同的SVM的性质。
参数空间可分为欠训练/过训练区和“好区”。以log C和log γ作为参数空间的坐标。经过大量实验证明，学习精度最高的参数组合将集中在“好区”中的直线log γ=log C-log N附近。
其中，对线性SVM求解最佳参数C,使其以其为参数的线性SVM学习精度最高，称之为N。<br>
<strong>7.2 网格搜索法</strong><br>
将C和γ分别取M个值和N个值，对M*N个组合，分别进行训练不同的SVM，再估计其学习精度，从而在M*N个组合中得到最高的一个组合作为最有参数。<br>
由上可知，网格法具有较高的学习精度，但是计算量大。而双线性法计算量小，但和网格法相比，学习精度较低。<br>
<br>
<strong>8 参数调优应用</strong><br>
在libSVM的tools文件夹里面包含了4个Python文件，是用来对参数优选的。其中最常用的是easy.py和grid.py。<br>
<strong>8.1 grid.py</strong><br>
grid.py是对C-SVC的参数C和γ做优选的，原理也是网格遍历。<br>

在libSVM的官方README的部分摘要如下：<br>
```
Part II: Parameter Selection Tools

Introduction
============

grid.py is a parameter selection tool for C-SVM classification using
the RBF (radial basis function) kernel. It uses cross validation (CV)
technique to estimate the accuracy of each parameter combination in
the specified range and helps you to decide the best parameters for
your problem.

grid.py directly executes libsvm binaries (so no python binding is needed)
for cross validation and then draw contour of CV accuracy using gnuplot.
You must have libsvm and gnuplot installed before using it. The package
gnuplot is available at http://www.gnuplot.info/

Usage: grid.py [-log2c begin,end,step] [-log2g begin,end,step] [-v fold]
       [-svmtrain pathname] [-gnuplot pathname] [-out pathname] [-png pathname]
       [additional parameters for svm-train] dataset

The program conducts v-fold cross validation using parameter C (and gamma)
= 2^begin, 2^(begin+step), ..., 2^end.

You can specify where the libsvm executable and gnuplot are using the
-svmtrain and -gnuplot parameters.

For windows users, please use pgnuplot.exe. If you are using gnuplot
3.7.1, please upgrade to version 3.7.3 or higher. The version 3.7.1
has a bug. If you use cygwin on windows, please use gunplot-x11.

Example
=======
> python grid.py -log2c -5,5,1 -log2g -4,0,1 -v 5 -m 300 heart_scale

Users (in particular MS Windows users) may need to specify the path of
executable files. You can either change paths in the beginning of
grid.py or specify them in the command line. For example,

> grid.py -log2c -5,5,1 -svmtrain "c:\Program Files\libsvm\windows\svm-train.exe" -gnuplot c:\tmp\gnuplot\binary\pgnuplot.exe -v 10 heart_scale

Output: two files
dataset.png: the CV accuracy contour plot generated by gnuplot
dataset.out: the CV accuracy at each (log2(C),log2(gamma))

Parallel grid search
====================

You can conduct a parallel grid search by dispatching jobs to a
cluster of computers which share the same file system. First, you add
machine names in grid.py:

ssh_workers = ["linux1", "linux5", "linux5"]

and then setup your ssh so that the authentication works without
asking a password.

The same machine (e.g., linux5 here) can be listed more than once if
it has multiple CPUs or has more RAM. If the local machine is the
best, you can also enlarge the nr_local_worker. For example:

nr_local_worker = 2

Example:

> python grid.py heart_scale
[local] -1 -1 78.8889  (best c=0.5, g=0.5, rate=78.8889)
[linux5] -1 -7 83.3333  (best c=0.5, g=0.0078125, rate=83.3333)
[linux5] 5 -1 77.037  (best c=0.5, g=0.0078125, rate=83.3333)
[linux1] 5 -7 83.3333  (best c=0.5, g=0.0078125, rate=83.3333)
.
.
.

If -log2c, -log2g, or -v is not specified, default values are used.

If your system uses telnet instead of ssh, you list the computer names
in telnet_workers.
```
*为了运行grid.py需要安装Python，并在path环境变量中进行添加路径。以及绘图工具gnuplot。
*找到grid.py文件。用python打开（不能双击，而要右键选择“Edit with IDLE”），修改svmtrain_exe和gnuplot_exe的路径。
```python
svmtrain_exe = r"D:\libSVM\program\svm-train.exe"
gnuplot_exe = r"D:\libSVM\gnuplot\pgnuplot.exe"
```
*运行cmd，进入dos环境，定位到grid.py的地方。
python grid.py heart_scale
你就会看到dos窗口中飞速乱串的[local]数据，以及一个gnuplot的动态绘图窗口。大约过10秒钟，就会停止。直接看最后一行：
2048.0 0.0001220703125 84.0741
其意义表示：C = 2048.0；γ=0.0001220703125；交叉验证精度CV Rate = 84.0741%，这就是最优结果。

*打开目录tools，我们可以看到新生成了两个文件：heart_scale.out和heart_scale.png，第一个文件就是搜索过程中的[local]和最优数据，第二文件就是gnuplot图像。

*grid.py运行完以后，你可以把最优参数输入到svmtrain中进行训练了。<br>

<strong>8.2 easy.py</strong><br>
文件easy.py对样本文件做了“一条龙服务”，从参数优选，到文件预测。因此，其对grid.py、svm-train、svm-scale和svm-predict都进行了调用（当然还有必须的python和gnuplot）。因此，运行easy.py需要保证这些文件的路径都要正确。当然还需要样本文件和预测文件。<br>



--------------------------------
######（转载本站文章请注明作者和出处 <a href="https://github.com/MangoLiu">MangoLiu</a> ，请勿用于任何商业用途）
