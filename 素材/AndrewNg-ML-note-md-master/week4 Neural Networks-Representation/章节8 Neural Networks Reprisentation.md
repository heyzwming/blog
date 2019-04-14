八、Neural Networks:Representation
===

## Motivations

## 54、Non-linear Hypotheses ( 非线性假设 )

看几个需要学习复杂的非线性假设的例子

假设有个监督学习分类问题，如果使用逻辑回归来解决这个问题，你可以构造出一个包含很多非线性项的逻辑回归函数，这里的g仍然是sigmoid函数，如果只有x1 x2两个特征，你确实可以用多项式的逻辑回归得到不错的结果，但事实上一般性的问题会有很多的特征。

现在假设你有一个包含3个特征量的训练集，现在你想要建立一个关于这个训练集的二次假设方程：
$$g(\theta_0+\theta_1x_1^2+\theta_2x_1x_2+\theta_3x_1x_3+\theta_4x_2^2+\theta_5x_2x_3+\theta_6x_3^2)$$

在这样的情况下，你需要两两配对各种情况，需要$​\frac{(3+2–1)!}{(2!⋅(3−1)!)}​$个二次项。

进一步，如果你有一个包含100个特征量的训练集，那么你需要​$\frac{(100+2–1)!}{(2⋅(100−1)!)}$=$5050​$个二次项。

我们继续推广，可以发现，二次项的空间复杂度为$​O(n^2/2)$​ ；如果再进一步推广到三次项，那么空间复杂度为$​O(n^3)​$ 。可以看到，当初始特征个数n很大时将这些高阶多项式项数包括到特征里会使特征空间急剧膨胀，当特征个数n很大时，增加特征来建立非线性分类器并不是一个好做法。这是一个非常陡峭的增长函数，如果我们需要处理大量特征量、高次假设方程，那么空间的增长将是巨大的。

因此，我们需要寻找一个替代的方法，优化如此巨大的复杂度。

![54.1](http://m.qpic.cn/psb?/V12umJF70r2BEK/51e73VLVCcwQR5B1.c5.gyTPr38Qr7NoT5n4LosVEE0!/b/dIUBAAAAAAAA&bo=pQULAwAAAAARF4g!&rf=viewer_4)

对于许多实际的机器学习问题，特征个数n是很大的。


对于一个图像识别问题中输入的图像是否是我们要识别的物体，我们需要一个非线性假设来区分开两类样本

![54.2](http://a2.qpic.cn/psb?/V12umJF70r2BEK/o2MvlvfOkI2x6Y1*LMbbkm9kDJpElERRUKXWg4q5V8I!/b/dA0BAAAAAAAA&ek=1&kp=1&pt=0&bo=lwUSAwAAAAARF6M!&tl=3&vuin=904260897&tm=1535421600&sce=60-2-2&rf=viewer_4)

只是包括平方项或者立方项特征简单的logistic回归算法并不是一个在n很大时学习复杂的非线性假设的好办法，因为特征过多

## 55、Neurons and the Brain ( 神经元与大脑 )

我们知道，目前大脑拥有最厉害的“机器学习”算法，那我们能否模仿它来实现更好的机器学习算法呢？神经网络可以在一定程度上模仿大脑的运作机制。这一领域早就已经有了很多概念，不过因为运算量大而难以进一步发展；近年来，随着计算机速率的提升，神经网络也得到了发展的动力，再次成为热点。

我们对于不同的数据处理需求可能会提出各种不同的算法，那么，有没有可能所有需求都由一个算法来实现，就像物理一样，人类追求一个大一统的“统一场论”呢？

科学家尝试做过这么一个实验：将大脑中听觉皮层与耳朵之间的神经连接切断，并且将听觉皮层与眼睛相连，结果发现听觉皮层可以“看到”东西。

![55.1](http://m.qpic.cn/psb?/V12umJF70r2BEK/wTVxMxPsgVRcA5gVALj**QNyJB*c2pqrMApJroKOgtA!/b/dN4AAAAAAAAA&bo=HAOlAQAAAAARB4s!&rf=viewer_4)

这说明，统一的学习算法是可能实现的。

## Neural Networks



## 56、Model Representation Ⅰ ( 模型展示 Ⅰ )

我们在应用神经网络的时候如何表示我们的假设或模型？

神经网络模仿了大脑中的神经元或者神经网络.

下面是一个简单的例子，图中的神经元是一个基本的运算单元，它由电信号从多个接受通道获取一定数目的信息输入(树突dendrites),并且计算后通过唯一的输出通道(轴突axon)给出输出。

![56.1](http://m.qpic.cn/psb?/V12umJF70r2BEK/g3RCoPG1dw7Wuzv379hTmENvJoknOGcqWV4*SA*9Vwk!/b/dN8AAAAAAAAA&bo=bgSgAgAAAAARB*g!&rf=viewer_4)

我们的神经网络模型，便是模仿了这一过程。

![56.2](http://m.qpic.cn/psb?/V12umJF70r2BEK/IOB2yFyXuHRWLEv1ILrvsbMVYT6blwWiA8V7*lTt1zc!/b/dN4AAAAAAAAA&bo=IwXZAgAAAAARF90!&rf=viewer_4)

黄色的圆圈是一个类似神经元细胞体的东西,然后我们通过树突或者说输入通道，传递给它一些信息,然后神经元做一些计算,通过输出通道输出假设函数$h_\theta(x)$计算结果

我们的**输入**是​$x_1⋯x_n$​ ，**输出**是假设函数。额外的，我们需要添加​$x_0=1$​，称作**偏置单元(bias unit)或偏置神经元**。

在神经网络的分类算法中，我们使用相同的逻辑函数$\frac{1}{1+e^{−θ^Tx}}​​$，有时候，我们称它为**逻辑激活函数**（Sigmoid/logistic activation function）。其中的**激活函数**（activation function）一般来说指“由……驱动的神经元计算函数$g(z)$”，像上例就是“由逻辑函数驱动的神经元计算函数$g(z)$”。

这里的参数$θ$也被称作**模型的参数**或**权重**（weight）。

在上面的模型中，红色的圆圈代表了一个神经元。在实际上，神经网络就是由不同的神经元连接组合而来的：

![56.3](http://m.qpic.cn/psb?/V12umJF70r2BEK/FZnR3uBS5abUpTLtUv6i2qJeMpWPSfsVTfu5kN.Nfu0!/b/dGwBAAAAAAAA&bo=IQXgAgAAAAARF.Y!&rf=viewer_4)

可以看到，我们的输入层（第一层）接收了数据的输入，计算后输出到输出层（第三层），但是这个模型中，中间还多了一层数据的处理，这一层的值不是x也不是y，是由输入层的数据加权组合后重新映射成的，称为**隐藏层（Hidden Layer）**。

一个神经网络不一定只有一个隐藏层

一个简单的表达形式如下：

$$\begin{bmatrix}
    x_0 \\ x_1 \\ x_2
\end{bmatrix} -> \begin{bmatrix}
    & & &
\end{bmatrix}  -> h_\theta(x)$$

一般而言，我们把隐藏层的节点或者说中间节点，称作**激活单元(activation units)**，并且有如下符号：

$a^{(j)}_i$=第j层的第i个激活单元  
$\Theta^{(j)}$=控制从第j层到j+1层的映射函数的权重矩阵

如果我们有一个隐藏层，那么整个流程看起来是这样的：

$$\begin{bmatrix}
    x_0 \\ x_1 \\ x_2 \\ x_3 
\end{bmatrix} ->
\begin{bmatrix}
    x_1^{(2)} \\ x_2^{(2)} \\ x_3^{(2)} 
\end{bmatrix} -> h_\theta(x)
$$

其中$x_1^{(2)}$是第2层的第1个激活单元


其中每一个激活节点的值是这样计算的：

$a^{(2)}_1=g(Θ^{(1)}_{10}x_0+Θ^{(1)}_{11}x_1+Θ^{(1)}_{12}x_2+Θ^{(1)}_{13}x_3)$

$a^{(2)}_2=g(Θ^{(1)}_{20}x_0+Θ^{(1)}_{21}x_1+Θ^{(1)}_{22}x_2+Θ^{(1)}_{23}x_3)$

$a^{(2)}_3=g(Θ^{(1)}_{30}x_0+Θ^{(1)}_{31}x_1+Θ^{(1)}_{32}x_2+Θ^{(1)}_{33}x_3)$

$h_Θ(x)=a^{(3)}_1=g(Θ^{(2)}_{10}a^{(2)}_0+Θ^{(2)}_{11}a^{(2)}_1+Θ^{(2)}_{12}a^{(2)}_2+Θ^{(2)}_{13}a^{(2)}_3)$

$\theta^{(1)}$就是控制着从三个输入单元到三个隐藏单元的映射的参数矩阵.  
$\theta^{(1)}$就是一个$3X4$的矩阵

也就是说，第$j$层的权重矩阵的每一行对应一个加权组合，然后通过$g(z)$函数映射到$j+1$层的节点。

![56.4](http://a4.qpic.cn/psb?/V12umJF70r2BEK/7PxOiPjEfPj6hm1nUz6HuzI0k1qOko79lplP48HYtV8!/b/dOsAAAAAAAAA&ek=1&kp=1&pt=0&bo=owUsAwAAAAARF6k!&tl=3&vuin=904260897&tm=1535446800&sce=60-2-2&rf=viewer_4)

由矩阵乘法可知：因此如果要从含有​$s_j$​个单元的第j层映射到含有$s_{j+1}$个单元的第$j+1$层，那么权重矩阵​$Θ(j)​$的尺寸为​$s_{j+1}×(s_j+1)$​，其中的+1是因为要考虑偏置单元​$x_0$​。


## 57、Model Representation Ⅱ ( 模型展示 Ⅱ )

向量化的实现方法


将原来的表示神经网络的方法进行简化  
我们通过左边的这些方程计算出三个隐藏单元的激活值,然后利用这些值来计算最终输出：假设函数h(x).

提醒以下：$a_1^{(2)}$是第二层神经网络的第一个激活单元值。

![57.1](http://a2.qpic.cn/psb?/V12umJF70r2BEK/CUGjY83atJrxTQC.NUlzrH4fbLP5wqlLU.smSShes1o!/b/dIUBAAAAAAAA&ek=1&kp=1&pt=0&bo=uAUWAwAAAAARF4g!&tl=3&vuin=904260897&tm=1535454000&sce=60-2-2&rf=viewer_4)

我们用一个变量​$z^{(j)}_k$​来表达$g(z)$函数的参数(例如$z^{(2)}_1$表示$Θ^{(1)}_{10}x_0+Θ^{(1)}_{11}x_1+Θ^{(1)}_{12}x_2+Θ^{(1)}_{13}x_3$)。其中$j$为第j层神经网络，$k$为该层神经网络的第k个激活单元值。

这些Z值都是线性组合，某个特定的神经元的输入值x0x1x2x3的加权线性组合.

现在，我们可以得到：

$a^{(2)}_1=g(z^{(2)}_1)$

$a^{(2)}_2=g(z^{(2)}_2)$

$a^{(2)}_3=g(z^{(2)}_3)$  
其中的z即为：

$$z^{(2)}_k=Θ^{(1)}_{k,0}x_0+Θ^{(1)}_{k,1}x_1+⋯+Θ^{(1)}_{k,n}x_n$$

可以看到函数z中包含一些与向量有关的信息,于是我们可以列出特征向量

输入的特征向量x和中间量​$z^{(j)}​$的向量表达为：

$$x = \begin{bmatrix}
    x_0 \\ x_1 \\ \vdots \\ x_n
\end{bmatrix}
z^{(j)} = \begin{bmatrix}
    z_1^{(j)} \\ z_2^{(j)} \\ ... \\ z_n^{(j)} \\
\end{bmatrix}$$

![57.2](http://m.qpic.cn/psb?/V12umJF70r2BEK/wmMu9VAtDK3MtHNU*a4*gx8SCosiYWarJGCGtpodSQI!/b/dOAAAAAAAAAA&bo=qwUJAwAAAAARF4Q!&rf=viewer_4)

进而，我们得到了参数z计算的表达式，计算第j-1层到第j层映射g(z)的参数z:

$$z^{(j)}=Θ^{(j−1)}x$$ (3.1)

例如
$$z^{(2)}=Θ^{(1)}x$$

下一步，我们可以计算第j层的节点激活值的向量(要知道的是，$z^{(2)}$和$a^{(2)}$都是三维向量)：

$$a^{(j)}=g(z^{(j)}) $$     (3.2)

例如

$$a^{(2)}=g(z^{(2)}) $$ 

我们可以把x向量写成$a^{(1)}$表示第一层的激活项的向量,然后把上面的$z^{(2)}$的公式替换掉。

$$z^{(j)}=Θ^{(j−1)}a^{(1)}$$

计算后，一定要记得添加偏置单元$​a_0^{(j)}​$ = 1.  
这样$a^{(2)}$就是一个四维特征向量。

我们用同样的方法计算下一层的信息：

$$z^{(j+1)}=Θ^{(j)}a^{(j)}$$

最后我们就要计算假设函数的实际输出值$h_\Theta(x)$，只需要计算$z^{(3)}$

$$z^{(3)}=Θ^{(2)}a^{(2)}$$

$$ h_\Theta(x) = a^{(3)}=g(z^{(3)})$$

这样的计算过程也称为前向传播。


如果j+1层是输入层，那么我们就得到了一个只有一行的输出：

$$ h_Θ(x)=a^{(j+1)}=g(z^{(j+1)})     $$ (3.3)

在计算过程中，一定不要用错下标。

这个神经网络所作的事情就像是逻辑回归,但它不是使用原本的x1x2x3作为特性，而是使用经过激活项计算后的a1a2a3作为新的特征,而a1a2a3是学习得到的函数输入值.具体说，是从第一层映射到第二层的函数,这个函数由其他参数$\Theta^{(1)}$决定。在神经网络中它没有用输入特征x1x2x3而是用自己训练的逻辑回归的输入a1a2a3..来训练逻辑回归

![57.3](http://m.qpic.cn/psb?/V12umJF70r2BEK/6jk6.vmF.fJdAuf8trp0g2Hn4U0O*bf5xcrUb9NIFl4!/b/dDwBAAAAAAAA&bo=cwULAwAAAAARF14!&rf=viewer_4)
神经网络中神经元的链接方式,称为神经网络的架构,架构是指不同的神经元的连接方式。



## Applications



## 58、Examples and Intuitions Ⅰ ( 例子与直觉理解 Ⅰ )

我们有两个特征$x_1$和$x_2$，他们是二进制形式的，只能取到0或1，左图的两个正样本和负样本可以看成右图复杂机器学习问题的简化版本。

![58.1]()

我们要做的就是要学习一个非线性的判断边界，来区分这些正样本和负样本。

我们要用神经网络来表达"与运算(AND)""或运算(OR)""异或运算(XOR)"

![58.2](http://a2.qpic.cn/psb?/V12umJF70r2BEK/9PhkD6IRWkMYiSk*d7uoPY3LGQ*Sykqupe3D3St7*ro!/b/dCEBAAAAAAAA&ek=1&kp=1&pt=0&bo=VwPXAQAAAAARF6I!&tl=3&vuin=904260897&tm=1535529600&sce=60-2-2&rf=viewer_4)

如左图所示，当$x_1$和$x_2$都为1或者都为0时$y=1$，当$x_1$或$x_2$中有一个为假，$y=0$.

下面我们先用一个简单的神经元来表达“逻辑与”，进行​$x1​$和$​x2​$的与运算。



我们有$x_1$,$x_2$,只能取到0和1，我们要求$y = x_1 \ AND \ x_2$的结果。

![58.2](http://m.qpic.cn/psb?/V12umJF70r2BEK/DV.kxhIixAfQm8RYawgKEu..XRMFdJ40CbTXGQ9EFCg!/b/dCEBAAAAAAAA&bo=VgPYAQAAAAARF6w!&rf=viewer_4)

模型的函数流程如下：

$$\begin{bmatrix}
    x_0 \\ x_1 \\ x_2
\end{bmatrix} -> \begin{bmatrix}
    g(z^{(2)})
\end{bmatrix} -> h_\Theta(x) $$ 

我们把第一层的权重矩阵设置为：


$$\Theta^{(1)} = \begin{bmatrix}
    -30 & 20 & 20
\end{bmatrix}$$

其中 -30 就是$\Theta^{(1)}_{10}$...


那么可以得到假设函数，之后我们验算结果：

$$h_Θ(x)=g(−30+20x_1+20x_2)$$

$$x_1=0 \ and \ x_2=0 \ then \ g(−30)≈0$$

$$x_1=0 \ and \ x_2=1 \ then \ g(−10)≈0$$

$$x_1=1 \ and \ x_2=0 \ then \ g(−10)≈0$$

$$x_1=1 \ and \ x_2=1 \ then \ g(10)≈1$$

结合逻辑回归的假设函数的函数图像可以看到这个神经元成功表达了逻辑与运算。

接下来看一下或运算函数的实现。

![58.3](http://m.qpic.cn/psb?/V12umJF70r2BEK/B81rG35hTgKXkH3rm49DJyXlXqjl0aeY2Y0SLMOihRE!/b/dCEBAAAAAAAA&bo=FANvAQAAAAARF1k!&rf=viewer_4)


实际运用中，就是这样不同的神经元层层连接，形成了用于复杂计算的神经网络（有点像电路中的运算元件）。

这里给出一组可以实现 与、或非、或的权重矩阵：

AND:  
$$Θ^{(1)} = \begin{bmatrix}
    -30 & 20 & 20
\end{bmatrix}$$

NOR:  
$$Θ^{(1)} = \begin{bmatrix}
    10 & -20 & -20
\end{bmatrix}$$

OR:  
$$Θ^{(1)} = \begin{bmatrix}
    -10 & 20 & 20
\end{bmatrix}$$





## 59、Examples and Intuitions Ⅱ ( 例子与直觉理解 Ⅱ )

接下来看一个实现“非运算的例子”

![59.1](http://m.qpic.cn/psb?/V12umJF70r2BEK/YtnQshXtVBubUKcvKJio5jmrwr7HDVqzNbbCzP.dyDk!/b/dCEBAAAAAAAA&bo=TQPYAQAAAAARF7c!&rf=viewer_4)

要实现逻辑非运算，大体思想上就是在预期而得到非结果的变量前面放一个很大的负权重。

在课件的右下角有一个思考题，如何实现($NOT$ $x_1$) $AND$ ($NOT$ $x_2$)

可以看到当且仅当$x_1=x_2=0$时$y=1$。

最后来看异或运算的实现。

![59.2](http://m.qpic.cn/psb?/V12umJF70r2BEK/lC8NUBHt5Y2Ud5RWi3QGGhMImLDtNjos5tfp*iPCd5U!/b/dCIBAAAAAAAA&bo=PwPtAQAAAAARF*A!&rf=viewer_4)

我们需要一个非线性的决策边界来分开正样本和负样本。

画出神经网络图，红线代表进行与运算，蓝色代表进行非运算，最后建立最后一个输出结点,绿色的部分代表进行或运算，当然我们仍然需要一个偏置单元.

当$x_1和x_2$都为0或1时$h_\Theta(x)$ 为 1.

![59.3](http://m.qpic.cn/psb?/V12umJF70r2BEK/Hqa0I8VdkfFAsyAt86UiMs3MukaaIgJdlFesaZUwLwc!/b/dCIBAAAAAAAA&bo=MgPPAQAAAAARF98!&rf=viewer_4)

当网络拥有许多层,在第二层重有一些关于输入的相对简单的函数,第三层又在此基础上计算更加复杂的方程,再往后计算的函数越来越复杂


## 60、Multiclass Classification ( 多元分类 )

我们上一章学过，要在神经网络重实现多类别分类，采用的方法本质上是一对多法的拓展，我们需要建立一个由多个输出单元的神经网络。

例如下面的例子，要识别出四种交通工具和行人，我们需要建立一个由四个输出单元的神经网络,这个神经网络的输出将是一个四维向量,然后用四个输出单元判断图片是否是某个物体。

![60.1](http://m.qpic.cn/psb?/V12umJF70r2BEK/SOm3ceSZadRYEB3M8GQNUlJat49kyCZqOYMz9uu5XyM!/b/dCIBAAAAAAAA&bo=OAPRAQAAAAARF8s!&rf=viewer_4)

对每一个类型拟合出一个假设函数：

$$\begin{bmatrix}
    x_0 \\ x_1 \\ x_2 \\ \dots\\ x_n
\end{bmatrix}
->
\begin{bmatrix}
    a_0^{(2)} \\
    a_1^{(2)} \\
    a_2^{(2)} \\
    \dots
\end{bmatrix}
->
\begin{bmatrix}
    a_0^{(3)} \\
    a_1^{(3)} \\
    a_2^{(3)} \\
    \dots
\end{bmatrix}
-> \dots ->
\begin{bmatrix}
    h_\Theta(x)_1 \\
    h_\Theta(x)_2 \\
    h_\Theta(x)_3 \\
    h_\Theta(x)_4 \\
\end{bmatrix}
->
$$

那么整合后的输出，可以用向量的形式表达：

$$h_\Theta(x) = \begin{bmatrix}
    0 \\ 0 \\ 1 \\ 0
\end{bmatrix}$$

在上面的例子中，分类为类型3的概率是1，是类型1、2、4的概率是0。

实际计算中，如果不同的类型都有概率，那么我们取概率最大的一个类型：

$$h_\Theta(x) = \begin{bmatrix}
    0.2 \\ 0.8 \\ 0.2 \\ 0
\end{bmatrix}$$

上面的例子中，分类为类型1、2、3、4的概率分别是0.2、0.8、0.2、0，我们取最大的概率0.8，对应的是类型2。
