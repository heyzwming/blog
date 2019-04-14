Octave/Matlab教程
===

## 基本操作

### 四则运算

```matlab
octave:1> 5+6   
ans =  11
octave:2> 3-2
ans =  1
octave:3> 5*8
ans =  40
octave:4> 1/2
ans =  0.50000
```

### 逻辑运算

```matlab
octave:6> 1 == 2        % 判等
ans = 0
octave:7> 1 ~= 2        % 非
ans =  1
octave:8> 1 && 0        % 与
ans = 0
octave:9> 1 || 0        % 或
ans =  1
octave:10> xor(1,0)     % 异或
ans =  1
```

### 其他

##### 更改提示符：

```matlab
octave:11> PS1('>> ')
>>
```

##### 添加分号可抑制输出：

```matlab
>> a = 1
a =  1
>> a = 1;
>>
```

##### 圆周率π：

```matlab
>> a = pi
a =  3.1416
```

##### 常数e：

```matlab
>> e
ans =  2.7183
```

##### 格式化输出：

```matlab
>> disp(sprintf('6 decimals: %0.6f', a))
6 decimals: 3.141593
>> disp(sprintf('6 decimals: %0.2f', a))
6 decimals: 3.14
>> format long 
>> a
a =  3.14159265358979
>> format short
>> a
a =  3.1416
```

### 矩阵
##### 构造一个矩阵，方式一：

```matlab
>> A = [1 2; 3 4; 5 6;]
A =

   1   2
   3   4
   5   6
```

##### 构造一个矩阵，方式二：

```
>> A = [1 2;
> 3 4;
> 5 6;
> ]
A =

   1   2
   3   4
   5   6
```

##### 构造一个横向量：

```matlab
>> v = [1 2 3]
v =

   1   2   3
```

##### 构造一个列向量：

```matlab
>> v = [1; 2; 3]
v =

   1
   2
   3
```

##### 从1到2，每次递增0.1

```matlab
>> v = 1:0.1:2
v =

 Columns 1 through 5:

    1.0000    1.1000    1.2000    1.3000    1.4000

 Columns 6 through 10:

    1.5000    1.6000    1.7000    1.8000    1.9000

 Column 11:

    2.0000
```

##### 从1到6，每次递增1(默认)：

```matlab
>> v = 1:6
v =

   1   2   3   4   5   6
```

##### 构造一个所有元素均为1的矩阵：

```matlab
>> ones(2, 3)
ans =

   1   1   1
   1   1   1
```

##### 每个元素乘以2，矩阵与标量乘法：

```matlab
>> C = 2*ones(2, 3)
C =

   2   2   2
   2   2   2
```

##### 高斯随机数：

```matlab
>> rand(3, 3)
ans =

   0.751588   0.906707   0.081204
   0.411613   0.457779   0.882052
   0.622524   0.774499   0.811092
```

##### 构造一个所有元素均为0的矩阵：

```matlab
>> w = zeros(1, 3)
w =

   0   0   0
```

##### 构造一个单位矩阵：

```matlab
>> I = eye(5)
I =

Diagonal Matrix

   1   0   0   0   0
   0   1   0   0   0
   0   0   1   0   0
   0   0   0   1   0
   0   0   0   0   1
   

A =

   1   2
   3   4
   5   6
```

##### 读取矩阵A第三行第二列的元素：

```matlab
>> A(3, 2)
ans =  6
```

##### 读取矩阵A第2列所有元素：

```matlab
>> A(:,2)
ans =

   2
   4
   6
```

##### 读取矩阵A第2行所有元素：

```matlab
>> A(2,:)   % ":" means every element along that row/column
ans =

   3   4
```

##### 读取矩阵A第1行和第3行的所有元素：

```matlab
>> A([1 3],:)
ans =

   1   2
   5   6
```

##### 将A第二列替换为[10;11;12]：

```matlab
>> A(:,2) = [10; 11; 12]
A =

    1   10
    3   11
    5   12
```

##### 在A的最后加上一列：

```matlab
>> A = [A, [100; 101; 102]]
A =

     1    10   100
     3    11   101
     5    12   102
```
##### 将A所有的元素合并成一个列向量

```matlab
>> A(:)
ans =

     1
     3
     5
    10
    11
    12
   100
   101
   102
```

##### 两个矩阵的合并（列合并）

```matlab
>> A = [1 2; 3 4; 5 6]
A =

   1   2
   3   4
   5   6

>> B = [7 8; 9 10; 11 12]
B =

    7    8
    9   10
   11   12

>> C = [A B]
C =

    1    2    7    8
    3    4    9   10
    5    6   11   12
```

##### 两个矩阵的合并（行合并）

```matlab
>> D = [A;B]
D =

    1    2
    3    4
    5    6
    7    8
    9   10
   11   12
```

##### 查看帮助：

```matlab
>> help eye
```

##### 构造10000个随机数，并绘制出图形(高斯分布)：

```matlab
>> w=randn(1,10000);
>> hist(w,50)
```

![高斯分布](http://a2.qpic.cn/psb?/V12umJF70r2BEK/EX3aCCa.DJBw2kDCtC6KimbMtEnJxswHXWURXZrYs6w!/b/dA0BAAAAAAAA&ek=1&kp=1&pt=0&bo=XgOhAgAAAAARF94!&tl=3&vuin=904260897&tm=1535274000&sce=60-2-2&rf=viewer_4)


## 移动数据(数据的读取与存储)
本节所用到的数据: featuresX.dat, priceY.dat

### 读取数据

```matlab
% 找到文件所在目录：
>> cd Desktop/
>> cd 'Machine Learning/'
>> ls
featuresX.dat	priceY.dat
```

数据如下所示：
![data]()

##### 读取数据：

```matlab
% 方式一：
>> load featuresX.dat
>> load priceY.dat
```

```matlab
% 方式二：
>> load('featuresX.dat')
>> load('priceY.dat')
```

##### 使用who命令显示当前所有变量：

```matlab
>> who
Variables in the current scope:

A          a          featuresX  v          y
C          ans        priceY     w
I          c          sz         x
```

##### 可以看到，刚才导入的数据已经在变量featuresX和priceY中了。

##### 展示数据：

```matlab
>> featuresX
featuresX =

   2104      3
   1600      3
   2400      3
   1416      2
   3000      4
   1985      4
   1534      3
    ...        ..

>> size(featuresX)
ans =

   27    2

>> priceY
priceY =

   3999
   3299
   3690
   2320
   5399
   2999
    ...
    
>> size(priceY)
ans =

   27    1
```

##### 使用whos查看变量更详细的信息：



##### 使用如下命令用来删除某个变量：

```matlab
>> clear featuresX
>> whos
```

##### 这个时候再使用whos查看，发现featuresX已经不见了。

### 存储数据
假设我们现在需要取出priceY前十个数据，使用如下命令：

```matlab
>> v = priceY(1:10)
v =

   3999
   3299
   3690
   2320
   5399
   2999
   3149
   1989
   2120
   2425
```   

##### 该如何存储这十个数据呢？使用save命令：

```matlab
>> save hello.mat v
>> ls
featuresX.dat	hello.mat	priceY.dat
```

##### 清空所有变量：

```
>> clear
>> whos
>> %无任何输出
```

###### 刚才存储数据是以二进制的形式进行存储，我们也可以使用人能够读懂的形式存储。例如：

```matlab
>> load hello.mat
>> whos
Variables in the current scope:

   Attr Name        Size                     Bytes  Class
   ==== ====        ====                     =====  ===== 
        v          10x1                         80  double

Total is 10 elements using 80 bytes

>> v
v =

   3999
   3299
   3690
   2320
   5399
   2999
   3149
   1989
   2120
   2425

>> save hello.txt v -ascii
>> ls
featuresX.dat	hello.mat	hello.txt	priceY.dat
```

## 计算数据

```matlab
% 各种矩阵运算
>> A * B    % 矩阵相乘
>> A .* B   % 将矩阵中所有对应位置的元素分别相乘， "."符号常用来做元素之间的运算
>> A .^ 2   % 对矩阵中的每一个元素进行乘方

>> v = [1; 2; 3]
v = 

    1
    2
    3

>> 1 ./ v   %   求得向量v的对应所有元素的倒数
>> 1 ./ A   %   求得矩阵A的对应所有元素的倒数 
>> log(A)   %   对v中所有的元素进行求对数运算
>> exp(A)   %   以e为底，以v中元素为指数的幂运算
>> abs(v)   %   求得v中所有元素的绝对值
>> -A       %   相反数
```
##### A中的每个元素都加上1：

```matlab
>> A + ones(size(A))
```

##### 这样也可以：

```matlab
>> A + 1
```

##### 矩阵转置：

```matlab
>> A'
```

##### 向量中的最大值：

```
>> A = [1 3 0.5 10 100]
A =

     1.00000     3.00000     0.50000    10.00000   100.00000

>> val = max(A)
val = 100
>> [val ind] = max(A)   % ind == index
val =  100
ind =  5
```

##### 比较大小：对每一个元素进行比较并返回含有布尔类型0、1的矩阵或向量

```matlab
>> A = [1 2; 3 4; 5 6]
A =

   1   2
   3   4
   5   6

>> A > 3
ans =

   0   0
   0   1
   1   1
```

##### 找出向量中特定元素：

```matlab
>> find(A > 3)  %找出所有元素中大于3的值索引
ans =

   3
   5
   6
```

##### 找出矩阵中特定元素并返回他们的所在行列(r c)

```matlab
>> [r c] = find(A >= 3)
r =

   2
   3
   2
   3

c =

   1
   1
   2
   2
```

##### 生成任意行、列、对角线和相等的矩阵：

```matlab
>> magic(3)
ans =

   8   1   6
   3   5   7
   4   9   2
```

##### 向量所有元素的和：

```matlab
>> a = [1.2 2.3 4.5 6.6]
a =

   1.2000   2.3000   4.5000   6.6000

>> sum(a)
ans =  14.600
```

##### 向量所有元素的乘积,用prod函数,prod == produce

```matlab
>> prod(a)
ans = ..

```

##### 向下及向上取整：

```matlab
>> floor(a)     % 向下取整，0.5舍去小数变成0
ans =

   1   2   4   6

>> ceil(a)      % 向上取整
ans =

   2   3   5   7
```

##### 构造一个随机矩阵

```matlab
>> rand(3)
ans =

   0.34033   0.33815   0.54004
   0.91126   0.58033   0.51210
   0.65947   0.35912   0.88464
```

##### 每个元素都是取两个随机矩阵最大的对应元素

```matlab
>> max(rand(3),rand(3))
ans =

   0.86455   0.86676   0.86657
   0.36503   0.92113   0.96915
   0.87765   0.59632   0.37732
```



##### 构造一个由A,B两个矩阵中对应位置较大的数组成的矩阵：


```matlab
>> A
A =

   1   2
   3   4
   5   6

>> B = [3 1; 4 6; 2 9]
B =

   3   1
   4   6
   2   9

>> max(A, B)
ans =

   3   2
   4   6
   5   9
```

##### 取出矩阵每列最大的元素：第一列最大为5，第二列最大是6，其中的1值得是从A的第一维度取值

```matlab
>> max(A, [], 1)
ans =

   5   6
```

##### 取出矩阵每行最大的元素：(第二维度)

```
>> max(A, [], 2)
ans =

   2
   4
   6
```

##### 想要直接获得矩阵中最大的元素，以下两种方式都可以：

```
% 方式一：
>> max(max(A))
ans =  6
% 方式二：
>> max(A(:))
ans =  6
```

##### 把A设为9X9的幻方

```matlab
>> A = magic(9)
A =

   47   58   69   80    1   12   23   34   45
   57   68   79    9   11   22   33   44   46
   67   78    8   10   21   32   43   54   56
   77    7   18   20   31   42   53   55   66
    6   17   19   30   41   52   63   65   76
   16   27   29   40   51   62   64   75    5
   26   28   39   50   61   72   74    4   15
   36   38   49   60   71   73    3   14   25
   37   48   59   70   81    2   13   24   35
```

##### 求A矩阵的每一列的和

```matlab
>> sum(A,1)
ans =

   369   369   369   369   369   369   369   369   369

```

##### 求A矩阵的每一行的和

```matlab
>> sum(A,2)
ans =

   369
   369
   369
   369
   369
   369
   369
   369
   369

```

##### 求对角线的和

```matlab
>> A .*eye(9)
ans =

   47    0    0    0    0    0    0    0    0
    0   68    0    0    0    0    0    0    0
    0    0    8    0    0    0    0    0    0
    0    0    0   20    0    0    0    0    0
    0    0    0    0   41    0    0    0    0
    0    0    0    0    0   62    0    0    0
    0    0    0    0    0    0   74    0    0
    0    0    0    0    0    0    0   14    0
    0    0    0    0    0    0    0    0   35
>> sum(sum(A .* eye(9)))
ans =  369
```


##### 矩阵的上下翻转：

```matlab
>> eye(3)
ans =

Diagonal Matrix

   1   0   0
   0   1   0
   0   0   1

>> flipud(eye(3))
ans =

Permutation Matrix

   0   0   1
   0   1   0
   1   0   0
```

##### 矩阵的逆：pinv(A)

```matlab
>> A = rand(3, 3)
A =

   0.68934   0.12881   0.80507
   0.49777   0.41907   0.37271
   0.32607   0.27877   0.41814

>> tmp = pinv(A)
tmp =

   1.795801   4.294380  -7.285421
  -2.180466   0.647802   3.620828
   0.053345  -3.780710   5.658801

>> tmp * A
ans =

   1.00000   0.00000   0.00000
   0.00000   1.00000   0.00000
  -0.00000  -0.00000   1.00000
```

## 数据绘制

> 绘图并可视化数据

##### 绘制出sin函数图像：横轴是x，纵轴是y

```matlab
x = [0: 0.01: 0.98];
>> y1 = sin(2*pi*4*x);
>> plot(x,y1);
```

##### 绘制出cos函数图像

```matlab
>> y2 = cos(2*pi*4*x);
>> plot(x,y2)
```

##### 将两个函数绘制在一起：使用hold on函数，即在旧的图像上绘制新的图像。带引号的r，指的是所使用的颜色

```matlab
>> plot(x,y);
>> hold on;
>> plot(x,y2,'r')
```

##### 添加说明：

```matlab
>> xlabel("time");      % xlabel 横轴标签
>> ylabel("value");     % ylabel 纵轴标签
>> legend("sin", "cos");   % 标记我的两条函数曲线

>> title("my plot");    % 标题
```

##### 存储图像：

```matlab
>> print -dpng "myPlot.png"
```

##### 关掉绘制的图像：

```matlab
>> close
```

##### 分别在两个窗口显示两个图像：

```matlab
>> figure(1); plot(x, y1);
>> figure(2); plot(x, y2);
```

##### 在同一窗口不同位置显示两个图像：subplot(1,2,1)将图像分成一个1*2的格子，并在第一个格子中绘图

```matlab
>> subplot(1,2,1);plot(x, y1);
>> subplot(1,2,2);plot(x, y2);
```

##### 改变左边图像的横坐标的刻度：

```matlab
>> subplot(1,2,1)
>> axis([0 0.5 -1 1])
```

##### 清除所有绘制的图像：

```matlab
>> clf
```

##### 将矩阵可视化：

```matlab
>> imagesc(magic(15))   % 不同颜色对应着矩阵中的不同的值

>> imagesc(A), colorbar, colormap gray  % 灰度颜色图和一个颜色条,使用逗号连续调用函数
```

![imagesc](http://m.qpic.cn/psb?/V12umJF70r2BEK/xja2xwopSb5ptwy7G3c8MUL3*63kMi8*V26cHKgTC5k!/b/dIIBAAAAAAAA&bo=MgIzAgAAAAARFyE!&rf=viewer_4)
![colorbar](http://m.qpic.cn/psb?/V12umJF70r2BEK/D6RXzVulD6FVeSVlOj2E1eXWB2eGCW3O1922WHzhgBo!/b/dN4AAAAAAAAA&bo=MgIzAgAAAAARFyE!&rf=viewer_4)

### 控制语句
##### for循环：

```matlab
>> v = zeros(10,1);
>> v
v =

   0
   0
   0
   0
   0
   0
   0
   0
   0
   0

>> for i = 1:10;
v(i) = 2^i;
end;
>> v
v =

      2
      4
      8
     16
     32
     64
    128
    256
    512
   1024

% 设置indices索引等于1到10
>> indices = 1:10;
>> indices
indices =

    1    2    3    4    5    6    7    8    9   10

>> for i = indices;
disp(i);
end;
 1
 2
 3
 4
 5
 6
 7
 8
 9
 10
>>

```

##### while循环：

```matlab
>> v
v =
      2
      4
      8
     16
     32
     64
    128
    256
    512
   1024

>> i = 1;
>> while i <= 5;
v(i) = 100;
i = i + 1
end;
i =  2
i =  3
i =  4
i =  5
i =  6
>> v
v =

    100
    100
    100
    100
    100
     64
    128
    256
    512
   1024
```

##### if 语句

```matlab
>> i = 1;
>> while true
     v(i) = 999;
     i = i + 1;
     if i == 6;
        break;
     end;
   end;
>> v
v =

    999
    999
    999
    999
    999
     64
    128
    256
    512
   1024
```

##### if-else 语句

```matlab
>> v(1)
ans =  999
>> v(1) = 2
>> if v(1) == 1,
     disp("The value is one")
   elseif v(1) == 2,
     disp("The value is two")
   else
     disp("The value is not one or two")
   end;
The value is two
>>

```



##### 定义一个函数：
你需要创建一个文件，然后用你的函数名命名，然后以.m为后缀

```matlab
function y1 = squareAndCudeThisNumber(x)

y = x^2
```

```matlab
>> ls
featuresX.dat		myPlot.png		squareThisNumber.m
hello.mat		octave-workspace
hello.txt		priceY.dat
>> squareThisNumber(3)
ans =  9
```

##### 如果该定义的函数不在当前目录下，我们就不能使用它：

```matlab
>> cd ~
>> pwd
ans = /Users/bobo
>> squareThisNumber(3)
error: 'squareThisNumber' undefined near line 1 column 1
```

##### 不过我们也可以更改Octave的搜索路径：

```matlabmatlab
>> addpath("~/Desktop/Machine-Learning")
```

##### 更改之后，我们现在虽然在/User/bobo目录下，但是仍然可以使用squareThisNumber函数。

```matlab
>> pwd
ans = /Users/bobo
>> squareThisNumber(5)
ans =  25
```

##### 返回两个值的函数：

```matlab
function [y1, y2] = squareAndCudeThisNumber(x)

y1 = x^2
y2 = x^3
```

```matlab
>> [y1, y2] = squareAndCudeThisNumber(3)
y1 =  9
y2 =  27
```

##### 代价函数：

![cost func](http://m.qpic.cn/psb?/V12umJF70r2BEK/eVFvCpbpzEHT41fYfUpZ38pT8zR347steFeniwhYdqU!/b/dOAAAAAAAAAA&bo=xQKqAQAAAAARB1w!&rf=viewer_4)

```matlab
>> X = [1 1; 1 2; 1 3]
X =

   1   1
   1   2
   1   3

>> y = [1; 2; 3]    %y轴对应的值
y =

   1
   2
   3

>> theta = [0; 1]
theta =

   0
   1

>> costFunctionJ(X, y, theta)
ans = 0
```

```matlab
>> theta = [0; 0]
theta =

   0
   0

>> costFunctionJ(X, y, theta)
ans =  2.3333
```

## 向量化