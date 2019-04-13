---
title: 吴恩达机器学习课程笔记（五）：Logistic Regression
date: 2019-03-29 11:15:00
tags: machinelearning
---

# Classification and Representation

## 5.1  分类(Classification)
 
> 预测值是离散值情况下的分类问题  

我们从预测值只有$0$、$1$两类的**分类问题**入手。

![5.1.1](assets/machinelearning/img/5.1.1.jpg)

在分类问题中应用线性回归不是一个好主意。因为一些额外的样本可能会影响线性回归函数的数据拟合度。

![5.1.2](assets/machinelearning/img/5.1.2.jpg)

对于分类问题，$y$的值是离散的$0$或$1$，如果使用线性回归，假设的输出值会远大于$1$或者小于$0$，即使所有的训练样本的标签都是$y=0$或$1$

我们要使用一种新的算法叫做**Logistac Regression**(逻辑回归，应用于分类问题)，特点在于算法的预测值的输出一直介于$0$和$1$之间。

## 5.2  假设陈述(Hypothesis Representation)

**Logistic Regression**中假设函数的表示方法:

我们希望我们的假设函数$h_\theta(x)$的输出能够在$0$到$1$之间。

当我们使用线性回归的时候，假设函数是这样的$h_\theta(x) = \theta^Tx$.其中$\theta$是参数的向量，$x$是特征值的向量，如下

$$
h_\theta(x) = \left[  \theta_0  .  \theta_1 \dots \theta_n  \right]
 \left[
 \begin{matrix}
   x_0  \\
   x_1  \\
   ·\\
   ·\\
   · \\
   x_n
  \end{matrix}
  \right] = \theta^T x$$

我们定义个函数 $g(z)=\frac{1}{1+e^{-z}}$ ,而$z = \theta^Tx$,这个函数$g$就叫**Sigmoid Function**或者**Logistic Function**，把$z$代入$h_\theta(x)$得到

$$h_\theta(x)=\frac{1}{1+e^{-{\theta^Tx}}}$$

我们要做的就是用参数$\theta$拟合我们的数据，拿到一个训练集，我们需要给参数$\theta$选定一个值，假设函数会帮我们做出预测。

![5.2.1](assets/machinelearning/img/5.2.1.jpg)


先解释下这个函数$h$输出的含义:

> 当函数$h$输出一个数字，我把这个数字当成:**对于一个输入x，y=1的概率估计**。

比方说$h_\theta(x) = 0.7$的意义是 (在单特征分类问题下)对于一个特征值为$x$的患者$y=1$的概率是$0.7$，用数学来表示，$h_\theta(x)$是 在特征值为x(此处是肿瘤的大小)和参数$\theta$的条件下，$y=1$的概率$p$(第$\theta$个特征是1，第1个特征是肿瘤大小)

假设函数$h_\theta(x)$的数学表达式是$P(y=1|x;\theta)$

![5.2.2](assets/machinelearning/img/5.2.2.jpg)


## 5.3  决策界限(Decision Boundary)  

**决策边界**的概念，能帮助我们更好的理解**Logistic Regression**的假设函数在计算什么。 

事实上这个假设函数计算的是 **在特征x和参数$\theta$的条件下$y=1$的估计概率**。

如果这个概率$p$>=0.5   

我们预测 $y = 1$    

否则 $y = 0$

看到这个图像我们可以看到如果$z$ >= 0 那么$g(z)$ >= $0.5$
即当$\theta^Tx >= 0$.我们的假设函数就会预测$y = 1$

![5.3.1](assets/machinelearning/img/5.3.1.jpg)

通过这些我们可以更好的理解**逻辑回归**的假设函数是如何做出预测的。

我们假设我们的假设函数是

$$h_\theta(x) = g(\theta_0+\theta_1x_1+\theta_2x_2)$$

为了拟合下图中的数据集，我们假设已经拟合好了参数$\theta$并有这么一个参数向量$\theta = \begin{bmatrix}
-3 \\ 1 \\ 1
\end{bmatrix}$, 来更深入理解假设函数何时为1何时为0

当$\theta^Tx=-3+x_1+x_2>=0$时预测$y=1$

$x_1+x_3 = 3$作出的区别开$y = 1$ 和$y = 0$范围的线就叫**决策边界(decision boundary)** 

![5.3.2](assets/machinelearning/img/5.3.2.jpg)


接下来是一个更加复杂的例子

我们添加额外的特征量$x_1^2$ 和$x_2^2$

我们选择参数向量$\theta=\begin{bmatrix}
    -1 & 0 & 0 & 1 & 1
\end{bmatrix}$ 

通过更复杂的多项式，我们可以得到更复杂的决定边界，决定边界不是训练集的属性，而是假设本身及其参数的属性，只要给定了参数向量$\theta$，圆形的决定边界就确定了。  

我们用训练集来拟合参数$\theta$

![5.3.3](assets/machinelearning/img/5.3.3.jpg)

最后再来看下更复杂 更高阶多项式的情况，这会让你得到非常复杂的决策边界。

![5.3.4](assets/machinelearning/img/5.3.4.jpg)



# Logistic Regression Model

## 5.4  代价函数(Cost Function)  

如何拟合logistic回归模型的参数$\theta$？

定义用来拟合参数的优化目标或者叫**代价函数**。

> 这就是logistic回归模型的拟合问题。

我们有一个m个训练样本的训练集${(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),···,(x^{(m)},y^{(m)})}$，每一个样本都用n+1维的特征向量表示。

$$x∈\begin{bmatrix}
  x_0 \\ x_1 \\ \dots \\ x_n
\end{bmatrix}  \ \ \ \ \ \ \ \ \ \  
x_0 = 1,y ∈\{0,1\} $$

对于给定的训练集我们如何选择 或者说如何拟合 参数$\theta$ ？

![5.4.1](assets/machinelearning/img/5.4.1.jpg)

我们定义$Cost(h_\theta(x),y)$，这个代价函数的理解是这样的，它是在输出的预测值是$h(x)$时而实际标签是$y$的情况下，我们希望学习算法付出的代价。

但是如果我们使用这个$Cost$代价函数，它会变成参数$\theta$的非凸函数。 

如果我们把$h_\theta(x) = \frac{1}{1+e^{\theta^Tx}}$代入$Cost(h_\theta(x),y)$中，再把$Cost$带入$J(\theta)$,会得到关于参数$\theta$的非凸函数。

![5.4.2](assets/machinelearning/img/5.4.2.jpg)

所以我们要找个别的代价函数

$$Cost(h_\theta(x),y) = \begin{cases}
-log{(h_\theta(x))} & y = 1 \\
-log(1-h_\theta(x)) & y = 0
\end{cases}$$

这个代价函数有一些有趣而且很好的性质  
* 如果假设函数的预测值是$1$，而且$y$刚好等于我的预测，那它的代价是$0$
* 如果预测值是$0$而$y = 1$则代价是$\infty$

![5.4.3](assets/machinelearning/img/5.4.3.jpg)

当$y=0$时同理

![5.4.4](assets/machinelearning/img/5.4.4.jpg)


## 5.5  简化代价函数与梯度下降(Simplified Cost Function and Gradient Descent)    

简化代价函数(将y = 0和y = 1两种情况合并)和如何用梯度下降法拟合出logistic回归的参数

#### Logistic regression cost function
$$J(\theta)=\frac{1}{m} \sum_{i = 1}^mCost(h_\theta(x^{(i)}),y^{(i)})$$

所有样本的代价和

$$Cost(h_\theta(x),y) = \begin{cases}
-log(h_\theta(x)) & y = 1 \\
-log(1-h_\theta(x)) & y = 0
\end{cases}$$

单个样本的代价

$Note$: $y$ = 0 or 1 always

将代价函数Cost简化：  
$$Cost(h_\theta(x^{(i)}),y^{(i)})= -y·log(h_\theta(x))-(1-y)·log(1-h_\theta(x))$$

通过将$y=1$或者$y=0$代入可证上式。

最后我们得到

$$J(\theta) = \frac{1}{m}\sum_{i=1}^m Cost(h_\theta(x^{(i)}),y^{(i)}) $$
$$= -\frac{1}{m}\begin{bmatrix}\sum_{i=1}^m  y^{(i)}·log(h_\theta(x^{(i)}))+(1-y^{(i)})·log(1-h_\theta(x^{(i)}))\end{bmatrix}$$

对这个代价函数，我们要拟合出参数$\theta$要做的是找出让$J(\theta)$取得最小值的$\theta$


![5.5.1](assets/machinelearning/img/5.5.1.jpg)

现在我们要获得最小化关于$\theta$的代价函数$J(\theta)$的方法.
如果你有n个特征，你就有一个参数向量$\theta = \begin{bmatrix}
\theta_0 \\ \theta_1 \\ \theta_2 \\ \vdots \\ \theta_n
\end{bmatrix}$

线性回归和Logistic回归的梯度下降算法的更新规则公式长得一模一样，但是其中的意义因为假设函数$h_\theta(x)$不同而不同

![5.5.2](assets/machinelearning/img/5.5.2.jpg)

另外，**特征缩放**的方法也可以让Logistic回归算法梯度下降收敛更快

## 5.6  高级优化(Advanced Optimization)  

换个角度来看梯度下降。

我们有代价函数并且想要最小化代价函数，当给定一个$\theta$，我们的代码需要计算出$J(\theta)$和偏导,梯度下降算法的作用是反复执行这些更新.

我们的思路是先用代码写出$J(\theta)$项和偏导项再代入到梯度下降算法中,然后它就可以为我们最小化这个函数

![5.6.1](assets/machinelearning/img/5.6.1.jpg)

当然除了梯度下降算法，我们还有一些别的算法

* 共轭梯度法(conjugate gradient )
* BFGS
* L-BFGS

这三种算法的优缺点

Advantages 

* No need to manually pick α
* Often faster than gradient descent 

Disadvantages

* More complex

调用高级算法的例子

![5.6.2](assets/machinelearning/img/5.6.2.jpg)



你要做的就是写要给函数，它能返回代价函数值以及梯度值，把这个应用于**Logistic回归**中。

![5.6.3](assets/machinelearning/img/5.6.3.jpg)


# Multiclass Classification

## 5.7  多元分类：一对多(Multiclass Classification:One vs all)  

在多类别分类问题中，$y$可以取多个离散值。例如

* Email foldering/tagging:Work(y=1),Friends(y=2),Family(y=3),Hobby(y=4)
* Medical diagrams:Not ill(y=1),Cold(y=2),Flu(y=3)
* Weather:Sunny(y=1),Cloudy(y=2),Rainy(y=3),Snow(y=4)

给出包含三个类别的数据集，我们如何得到一个学习算法进行分类呢？

![5.7.1](assets/machinelearning/img/5.7.1.jpg)


接下来介绍一对多分类的原理：

我们有一堆数据集，有三个分类，用三角形表示$y=1$，正方形表示$y=2$，叉叉角标$y=3$

我们要做的就是把这个训练集转化为三个独立的二元分类问题  

我们将创建一个新的“伪”训练集，其中类别2和类别3设定为负类，类别1设定为正类，我们要拟合一个分类器，我们称其为$h_\theta^{(1)}(x)$,然后进行正常的二元分类问题。

再对其他类别进行同样的操作

我们有三个分类器，每个分类器都针对其中一种情况进行训练。

![5.7.2](assets/machinelearning/img/5.7.2.jpg)

我们训练了一个逻辑回归分类器$h_\theta^{(i)}(x)$，为每一个i类别预测出一个y=i的概率，最后为了做出预测，我们给出一个新的输入值x期望获得预测，我们输入一个x，然后选择h最大的类别，也就是选择出三个中可信度最高效果最好的分类器，我们就能得到一个最高的概率，我们预测的y就是那个概率。

![5.7.3](assets/machinelearning/img/5.7.3.jpg) 



