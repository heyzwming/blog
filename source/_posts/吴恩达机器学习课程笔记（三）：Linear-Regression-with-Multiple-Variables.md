---
title: 吴恩达机器学习课程笔记（三）：Linear Regression with Multiple Variables
date: 2019-03-29 11:10:00
tags: machinelearning
---

## 3.1、 Multiple Features(多特征)

假设我们有多个特征变量和更多可以用来预测价格的信息，我们使用$x_1$ 、 $x_2$ 、 $x_3$ 、 $x_4$来表示我们的四个特征，$y$来表示我们想要预测的房屋的输出变量(价格)。

$m$：样本的数量

$n$：特征的数量

$x^{(i)}$：第$i$个训练样本的输入特征值 

> 注意： $x^{(i)}$是个向量，存放着所有特征量的值

$x_j^{(i)}$：第$i$个训练样本中第$j$个特征量的值


![3.1.1](/assets/machinelearning/img/3.1.1.jpg)

我们有多个特征量的假设的形式应该是怎样的？

$$h_\theta(x) = \theta_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n$$


![3.1.2](/assets/machinelearning/img/3.1.2.jpg)

为了符号方便，我们定义了额外的第$\theta$个特征向量$x_0$，并且它的取值总是$1$。

$$h_\theta(x) = \theta_0x_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n$$

我们发现由$\theta$向量的转置 **内积** $x$向量的形式，为我们提供了一个便利的方式来表示假设函数$h_\theta(x)$

$$h_\theta(x) = \theta_0x_0 + \theta_1x_1 +...+ \theta_nx_n = \theta^Tx $$

![3.1.3](/assets/machinelearning/img/3.1.3.jpg)

这就是在多特征量的情况下的假设形式，它就是所谓的**多元线性回归**(Multivariate linear regression)。


## 3.2、 Gradient Descent for Multiple(多元梯度下降法)

我们要利用**向量化**将所有的参数全部**同时更新**。

我们把参数$\theta$看成是一个有关$\theta$的 n+1 维向量， $J$ 看成 参数$\theta$这个向量的函数。将之前的假设函数和参数还有代价函数和梯度更新公式全部更新为用向量形式表示。

![3.2.1](/assets/machinelearning/img/3.2.1.jpg)

我们的梯度下降算法也得到了更新：用于**多元线性回归的梯度下降算法**。

我们通过向量化方法将所有的有多个特征变量的更新式子整合到一个式子中,实现一步更新所有参数变量的目的。

![3.2.2](/assets/machinelearning/img/3.2.2.jpg)

## 3.3、Gradient Descent in Practice Ⅰ - Feature Scaling(多元梯度下降法演练 I – 特征缩放) 

你有一个机器学习问题，这个问题有多个特征，如果你能确保这些特征都在一个相近的范围，这样特度下降算法就可以更快地收敛。

也就是说，如果你的两个特征量(假设你有两个特征量)的值相差很大，那么，梯度下降算法收敛的速度可能会因为发生**震荡**而进度很慢，如下左图。

一种有效的解决方法就是进行**特征缩放**， 使得$x_1$和$x_2$都在0-1的范围，这样得到的梯度下降算法就会更快地收敛。

![3.3.1](/assets/machinelearning/img/3.3.1.jpg)

当然， 你认可的范围可以超过1和-1但别太大

![3.3.2](/assets/machinelearning/img/3.3.2.jpg)

除了将特征值除以最大值外，还有一种方法叫做 **均值归一化(Mean normalization)** 

用$x_i-μ_i$($μ_i$是特征值的取值平均值)来代替$x_i$，让你的特征值具有为$0$的平均值

更一般的，你可以把$x_1$替换为$\frac{x_1-μ_1}{s_1}$  ($s_1$是该特征值的范围，max-min)，也可以把$s_1$设为变量的标准差。

![3.3.3](/assets/machinelearning/img/3.3.3.jpg)

## 3.4、Gradient Descent in Practice Ⅱ - Learning Rate(多元梯度下降法II – 学习率)

* 如何选择学习率$α$
* debug是什么
* 一些小技巧来确保梯度下降算法是正常工作的

![3.4.1](/assets/machinelearning/img/3.4.1.jpg)

**梯度下降算法**所做的事情是为你找到一个$\theta$值，并且希望它能够最小化代价函数$J(\theta)$

> 我经常做的事是在算法运行的时候列出$J$的值

下图 ( 代价函数$J(\theta)$随迭代次数的变化曲线 ) 的**x轴**是梯度下降算法的**迭代次数**，随着梯度下降算法的运行，你会得到如下的曲线，**y轴**就是经过x次迭代后得到的$\theta$算出的**代价**：$J(\theta)$值。

如果在迭代进行过程中，每一步迭代之后$J(\theta)$的值都应该在下降。   
这条曲线可以帮助你判断梯度下降算法是否已经收敛。

> 不同的问题梯度下降算法的迭代步数可能大相径庭。

当然我们可以通过一个自动收敛测试算法来得到是否收敛的结果。当然还是看左边的图像更方便一点，因为图像可以更直观地看出算法有没有正常工作。

![3.4.2](/assets/machinelearning/img/3.4.2.jpg)

图像的小$Tips$：

    如果出现：  
    1、 上升  
    2、 正弦式震荡

solution ：**减小$α$值**

![3.4.3](/assets/machinelearning/img/3.4.3.jpg)

为了调试所有的情况，通常绘制$J(\theta)$随迭代步数变化的曲线可以帮你弄清楚到底发生了什么。

选择一个使得$J(\theta)$快速下降的$α$值：吴恩达教授表示可以先取一些较小的值，如0.0001，然后三倍三倍增长，在最大的$α$和极小的之间取最大偏小一点的那个$\alpha$。

![3.4.4](/assets/machinelearning/img/3.4.4.jpg)



## 3.5、Features and Polynomial regression(特征和多项式回归)

使用**多项式回归**可以使 *用线性回归的方法* 拟合非常复杂的函数，甚至是非线性函数。

还是房价预测问题，我们有两个特征：**临街宽度(frontage)**和**纵深(depth)**，分别设为$x_1$和$x_2$。  

当然我们也可以自定义出一些其他的特征，房屋出售的价格其实取决于你拥有的土地的大小。

于是我们创造一个新的特征$x$，即临街宽度与纵深的乘积。只用这个特征(土地面积)写假设函数。

> 有的时候通过**定义一个新特征**可能会使你的模型拟合的更好。

![3.5.1](/assets/machinelearning/img/3.5.1.jpg)

这就叫**多项式回归(poly regression)**。

有时一次函数和二次函数$(ax^2+bx+c)$都不能很好的拟合数据，我们可能会考虑到多项式$(ax^3+bx^2+cx+d)$的回归。

那么我们如何将模型和数据进行拟合呢？

通过

$x_1$ = $size$

$x_2$ = $size^2$

$x_3$ = $size^3$

并转化成 $h(x) = \theta_0+\theta_1x_1 +\theta_2x_2 + \theta_3x_3$的形式

![3.5.2](/assets/machinelearning/img/3.5.2.jpg)

此时的**特征缩放**就显得更重要了

## 3.6、Normal Equation(正规方程（区别于迭代方法的直接解法）)

对于某些线性回归问题**正规方程**会给我们更好的方法来求得参数$\theta$的最优值$\theta$

**正规方程**提供了一种求$\theta$的**解析解法**(Method to solve for $\theta$ analytically),我们可以直接**一次性**地求解$\theta$的最优值。

假设$\theta$是一个一维标量，也就是实数，要求$J(\theta)$函数在局部最小值时的$\theta$，可以对$J(\theta)$**求导**，并置$\theta$，得到使得$J(\theta)$最小的$\theta$值。

假设$\theta$是多个值(一个向量)呢？就对每一个$\theta$求偏导并置$\theta$得到$\theta$的值。

![3.6.1](/assets/machinelearning/img/3.6.1.jpg)


> 如果你使用正规方程法，你就不需要进行特征缩放，即使多个特征之间的取值范围相差很大。

> 但是如果你使用梯度下降法求$\theta$你仍然需要使用特征缩放。

![3.6.2](/assets/machinelearning/img/3.6.2.jpg)

$$ X = \begin{bmatrix} 
  x_0^{(1)} & x_1^{(1)} & ... & x_n^{(1)} \\

  x_0^{(2)} & x_1^{(2)} & ... & x_n^{(2)} \\

  \vdots & \vdots & \vdots & \vdots \\

  x_0^{(m)} & x_1^{(m)} & ... & x_n^{(m)} \\

   \end{bmatrix}  $$

   $$m * (n+1)$$

$$ y = \begin{bmatrix}
  y^{(1)}\\
  y^{(2)}\\
  \vdots \\
  y^{(m)}\\
\end{bmatrix}$$
  $$m*1$$

$$\theta = (X^TX)^{-1}X^Ty$$

![3.6.3](/assets/machinelearning/img/3.6.3.jpg)

![3.6.4](/assets/machinelearning/img/3.6.4.jpg)

## 最后
> 你何时应该使用梯度下降算法，何时应该使用正规方程法？
> 

当你有m个训练样本，和n个特征。

对于**梯度下降算法**，你需要选择**学习速率$\alpha$**，而且它需要更多次的迭代，计算过程可能很慢。

但是梯度下降算法在特征量很多的情况下也能运行地相当好，而正规方程法需要计算相关的逆矩阵，而在计算机中计算这个n X n 逆矩阵的代价是**O(n^3)**，n是矩阵的维度，即特征量的数量。

通常在n大于10^4 可以考虑使用梯度下降算法

![3.6.5](/assets/machinelearning/img/3.6.5.jpg)

## 总结一下：
只要特征变量的数目并不大正规方程是一个很好的计算参数$\theta$的替代方法。

