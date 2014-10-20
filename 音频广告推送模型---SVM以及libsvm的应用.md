#音频广告推送模型---SVM以及libsvm的应用
--------------------------------
##SVM
本文需要有SVM的理论知识背景，若是对SVM不熟悉，请先参考：<br>
`《数据挖掘导论》`(人民邮电出版社)，相对简单入门。<br>
`《统计学习方法》`(清华大学出版社)，相对深入数学原理。<br>
`《机器学习实战》`(人民邮电出版社)，相对偏重应用(python编写)。<br>
三本书中都有涉及SVM的基本知识。而且讲的不错。<br>
再额外推荐下july写的[支持向量机通俗导论(理解SVM的三层境界)](http://blog.csdn.net/v_july_v/article/details/7624837)

## libSVM
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

<br>-----------<br>
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



--------------------------------
######（转载本站文章请注明作者和出处 <a href="https://github.com/MangoLiu">MangoLiu</a> ，请勿用于任何商业用途）
