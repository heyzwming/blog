一、Introduction(绪论：初识机器学习)
===

## 前言

> 吴恩达教授（Andrew Ng）的《机器学习》课程目前流传着两个版本，一个是早期的斯坦福大学的公开课（CS299），这个版本录制于吴恩达教授在斯坦福大学任教时期，课时较长，理论内容也更为深入；另一个是目前Coursera上更新的网课，课时较短，配有较多programming exercise。一般认为，Coursera版本更加适合快速入门，斯坦福版本适合进阶深究。

> 本文的定位主要是归纳总结，以学习时的思路展开，以吴恩达教授的课程内容为主体。

## Introduction
    introduce the core idea of teaching a computer to learn concepts using data—without being explicitly programmed.

    We are going to start by covering linear regression with one variable. Linear regression predicts a real-valued output based on an input value. We discuss the application of linear regression to housing price prediction, present the notion of a cost function, and introduce the gradient descent method for learning.


## 1.1、Welcome (欢迎参加《机器学习》课程) 

pass

## 1.2、What is Machine learning (什么是机器学习?)

Two definitions of Machine Learning are offered. 

1、**Arthur Samuel** described it as: 
> "the field of study that gives computers the ability to learn without being explicitly programmed." This is an older, informal definition.

**Arthur Samuel**认为，机器学习致力于让计算机在**没有明确地编程任务设置**的情况下使计算机有学习能力。这是一种**老旧**、**不正式**的定义。（他曾让电脑自己进行双方博弈，通过观察判断哪种棋局的布局赢面更大。通过这样的方法，最终得到了一个厉害的下棋程序。）

2、**Tom Mitchell** provides a more modern definition: 
> "A computer program is said to learn from **experience E** with respect to some class of **tasks T** and **performance measure P**, if its performance at tasks in T, as measured by P, improves with experience E."

**Tom Mitchell**认为，从“ 以**某种任务T为目标**，以**P为性能指标**的**经验E**中 ”进行学习的计算机程序，它在“以任务T为目标，P为性能指标”的情况下，表现会随着“经验E”的增长而变得更好。**通过P测定在T上的表现因经验E而提高**。Andrew Ng认为这是一个和现在比较相符的定义（尽管他开玩笑Tom Mitchell是为了押韵才这样表述）。


**Example**: playing checkers.

> **E** = the **experience** of playing many games of checkers.玩多局游戏得到的经验  
> **T** = the **task** of playing checkers. 跳棋取胜的目标  
> **P** = the **probability** that the program will **win** the next game.程序赢得游戏的可能性

In general, any machine learning problem can be assigned to one of two broad classifications:

> `Supervised learning` and `Unsupervised learning`.

> 
    Machine learning algorithms:
        - Supervised learning(监督学习)
        - Unsupervised learning(无监督学习)
    Others:
        - Reinforcement learning(强化学习)
        - recommender systems(推荐系统)

习题1.1 Suppose your email program watches which emails you do or do not mark as spam, and based on that learns how to better filter spam. What is the task T in this setting?

* Classifying emails as spam or not spam.   –T

* Watching you label emails as spam or not spam.     -E

* The number(or fraction) of emails correctly classified as spam/not spam.  -P



## 1.3、Supervised Learning (监督学习)

> In supervised learning, we are given a data set and **already know what our correct output should look like**, having the idea that there is a relationship between the input and the output.

在监督学习中，我们已经得到了一个数据集，并且已经知道正确的输出是什么样子，而且预设输入和输出之间一定存在某种联系。

Supervised learning problems are categorized into **"regression(回归)"** and **"classification(分类)"** problems. 

- In a **regression problem**, we are trying to predict results within a **continuous output**, meaning that we are trying to map input variables to some **continuous function(连续的函数)**. 

- In a **classification problem**, we are instead trying to predict results in a **discrete output**. In other words, we are trying to map input variables into **discrete categories(离散的类别)**.

---
Example 1:

给定一些真实房地产(estate)市场的一些关于房屋价格与面积的数据，尝试用这些数据来预测房子的价格，房子的价格和房子的面积相关的函数是一个**连续的输出**，所以这是一个**回归问题(Regression problem)**。

在例子中，横轴(horizontal axis)是不同房屋的平方英尺数,纵轴(vertical axis)是不同房子的价格，单位是千美元,如果你进行的是线性回归，(或者说用一条直线拟合数据)那么面积为750的房子，得到的是150k的价格，但是，线性模型似乎与实际数据的拟合度不是特别好。如果采用二次函数进行拟合，可以看到拟合度更高了，因此得到了200k的价格。这样看来，回归问题的模型选取是十分重要的。

![例1](http://m.qpic.cn/psb?/V12umJF70r2BEK/8g*Z74GVKRK9kgfmWaUhtLCtfXpzuLitdWfdCMlvYRw!/b/dJABAAAAAAAA&bo=cgXwAgAAAAARB7U!&rf=viewer_4)

> And the term(术语) supervised learning refers to the fact that we gave the algorithm **a data set**, in which **the "right answers" were given**.
And the task of the algorithm was to just **produce more** of these right answers.
>
> This is alse called a regression problem.And by regression problems I mean,we're trying to predict a **continuous valued output(连续的数值输出)**.

The other way,we could turn this example into a **classification problem(分类问题)** by instead making our output about whether the house "sells for more or less than the asking price." Here we are classifying the houses based on price into two discrete(离散的) categories(分类).

---
Example 2:

> (a) **Regression** - Given a picture of a person, we have to predict their age on the basis of the given picture
>
> (b) **Classification** - Given a patient with a tumor, we have to predict whether the tumor is malignant or benign.

我们得到了一些乳腺癌(breast cancer)关于（肿瘤尺寸，良性(benign)/恶性(malignant)）的真实数据，现在要根据某一肿瘤的尺寸，来判断预测乳腺癌是否是良性的。在这个例子里，输出是Yes or Not, 是一个**分类问题**。

在这个例子中，横轴是肿瘤的尺寸(Tumor Size)，纵轴是1或0，代表是或否(on your horizontal axis the size of the tumor and on the vertical axis I'm going to plot one or zero, yes or no)，即我们看到的肿瘤样本是否是恶性(malignant)的。

如果有一个朋友的肿瘤大小在这个(紫色箭头)处，机器学习的问题就是，你能否估计出肿瘤是良性的还是恶性的概率。

![例2](http://m.qpic.cn/psb?/V12umJF70r2BEK/BiejrgPXXWmnYeVnIlZ1wCmw8t*r7Wmn8nTh8AXpepw!/b/dOAAAAAAAAAA&bo=nwXiAgAAAAARF1o!&rf=viewer_4)

> To introduce a bit more terminology,this is an example of a classification problem. The term classification refers to the fact that here we're trying to predict a discrete valued output.

用更专业的术语来说,这是一个分类问题，分类是指我们设法预测一个**离散值输出**。

显然这个例子只是简单的模型，实际问题中的变量要复杂的多,你可能有两个以上的可能输出值，比如肿瘤性质划分更细：良性、I类、II类、III类等；以及肿瘤的样本特征：厚薄、大小、形状等；甚至患者的年龄等。分类算法要做的是划一条线把他们分离开来。

在分类问题中，你可以用另一个方法来绘制这些数据(图的下半部分),用不同的符号来表示良性( O )或恶性( X )。

![例3](http://m.qpic.cn/psb?/V12umJF70r2BEK/v1FCtNnd1uEvyB*aux0hzRaRmQ9PPq.GTVDcm.mBoig!/b/dIQBAAAAAAAA&bo=jQXcAgAAAAARF3Y!&rf=viewer_4)

> Now, in this example we use only one feature or one attribute,namely,the tumor size,in order to predict whether the tumor is malignant or benign.In other machine learning problems,when we have more than one feature ,more than one attribute.
>
 在这个例子中，我们只使用了一个特征或者说属性，即肿瘤的大小来预测肿瘤是恶性的还是良性的。在其他的机器学习问体中,我们会有多个特征，多个属性。

当有有多个特征(如下图中的Tumor Size 和 Age, 或者更多如肿块的厚度(Clump Thickness)、癌细胞大小的均匀性(Uniformity of Cell Size)、癌细胞形状的均匀性(Uniformity of Cell Shape))的数据集时，可以如下的形式来绘制。

![例4](http://m.qpic.cn/psb?/V12umJF70r2BEK/wk1goOCewVdQieOUgxi8dCAO5ebvaYIBz.WmhFU528k!/b/dAkBAAAAAAAA&bo=cgW1AgAAAAARF.A!&rf=viewer_4)

> So, just to recap, in this course, we'll talk about Supervised Learning, and the idea is that in Supervised Learning, in every example in our data set, we are told what is the correct answer that we would have quite liked the algorithms have predicted on that example. Such as the price of the house, or whether a tumor is malignant or benign. We also talked about the regression problem, and by regression that means that our goal is to predict a continuous valued output. We talked about the classification problem where the goal is to predict a discrete value output.

概括一下，在这节课上我们讨论了监督学习，想法是在监督学习中，对于训练数据集中的每个样本，我们都已经给出了相应的正确答案。我们想要算法预测并得出“正确答案”，像房子的价格，或肿瘤是恶性的还是良性的。

我们也讨论了回归问题,回归是指我们的目标，是预测一个连续值输出,我们还讨论了分类问题，其目的是预测离散值输出



习题2.1 You’re running a company, and you want to develop learning algorithms to address each of two problems.

Problems1: You have a large inventory of identical items. You want to predict how many of these items will sell over next 3 months.

Problems2: You’d like software to examine individual customer accounts, and for each account decide if it has been hacked/compromised.

Should you treat these as classification or as regression problems?

---

## 1.4、Unsupervised Learning

> Unsupervised learning allows us to approach(处理) problems **with little or no idea** what our results should look like. We can derive(得到) structure from data where we don't necessarily know the effect of the variables.

无监督学习，使我们能够处理那些对结果**了解甚少、甚至根本不了解**的问题。我们可以在不知道变量的具体影响的情况下，从数据集中提取出数据的结构（structure）。

> We can derive(得到) this structure by clustering(聚集) the data based on relationships among the variables in the data.

我们得到一个数据集，我们不知道要拿它来做什么，也不知道每个数据点究竟是什么,我们只被告知这里有一个数据集,这些数据没有标注也没有labels，你要在其中，利用数据之间的变量关系和特征，用聚类的方法找到并提取出某种结构。

无监督学习不能从预测的结果中得到反馈（没有性能测度），也就是说，没有老师来纠正你，也没有所谓的"正确答案"。

![3.0](http://m.qpic.cn/psb?/V12umJF70r2BEK/GFsL7WiSiBUzxjQjanISbAytLyFcNt..KM1dJtorIgw!/b/dNoAAAAAAAAA&bo=XwT1AgAAAAARB5w!&rf=viewer_4)


Example:

> **Clustering**: Take a collection of 1,000,000 different genes(基因), and find a way to automatically group these genes into groups that are somehow similar or related by different variables, such as lifespan, location, roles, and so on.

例3.1 基因测定问题。在这个例子中，你得到了一系列的个体的DNA微阵列数据，对于每个个体，检测他们是否拥有某个特定的基因，也就是要检测特定基因的表达程度，用不同颜色标记了它们特定基因序列的拥有程度。现在通过聚类算法，你能把这些个体归入不同的类里。

![3.2->1](http://m.qpic.cn/psb?/V12umJF70r2BEK/duvdM0gbsDoBMU7.hOlIo7MGTPbtDRY0yDwATKO.9og!/b/dN4AAAAAAAAA&bo=sQXHAgAAAAARF1E!&rf=viewer_4)


例3.2 我们进入谷歌新闻，可以看到BP油井事故的专题，有华尔街日报、CNN、卫报。在这个例子中，谷歌通过搜索成千上万条新闻，再自动将数据集聚类，把相关内容显示到一起。

![3.1->2](http://m.qpic.cn/psb?/V12umJF70r2BEK/91lWDyUuwXeqzcKTJgzZsQuQhnLKhpoo7sBR8PdxNi0!/b/dN8AAAAAAAAA&bo=cQU0AwAAAAARF2M!&rf=viewer_4)

例3.3 类似的还有如下问题：依照处理的问题不同，把计算机集群聚类，放置在一起优化效率；依照社交网络如Facebook上的互动，通过聚类判断他们是否属于同一个社交圈；根据大量的客户信息，将客户归入不同的细分市场，有针对性的推送广告；在天文学上，通过聚类得出了有意思的星系成型理论。

![3.3](http://m.qpic.cn/psb?/V12umJF70r2BEK/Nx7VuT*KuZXHYXq73VrqP0eCWZhpPjmfqvY9I6jVsLY!/b/dAoBAAAAAAAA&bo=cwUlAwAAAAARF3A!&rf=viewer_4)

> **Non-clustering**: The "Cocktail Party Algorithm", allows you to find structure in a chaotic environment. (i.e. identifying individual voices and music from a mesh of sounds at a cocktail party).

例3.4 鸡尾酒会问题也是一个典型问题。在喧闹的鸡尾酒会上，大脑的集中一个人的听觉能力关注特定的刺激，同时过滤掉范围内的其它刺激。

![3.4](http://m.qpic.cn/psb?/V12umJF70r2BEK/7K*1jrS5nSVGEcVlld6IxhvMZ2iotG7B4IBijKO4SZ4!/b/dGwBAAAAAAAA&bo=uAT9AgAAAAARF2M!&rf=viewer_4)

现在我们把模型简化一下，假定有两个人用不同的语言在从一数到十，两个麦克风在收录。那么对麦克风而言，传输给它们的都是两个说话者的叠加。但依照聚类算法，我们可以将使得两个麦克风分别关注一个谈话者，从而将他们的说话信息分离出来。

## 5、Review
