# SageMath简明教程



 <font size="6" face="仿宋" >目录</font> 

------

[TOC]

<div style="page-break-after:always"></div>

## 序言

笔者在编写USTC信息安全实践课程的二级密码学讲义中，简要编写了SageMath的入门教程，主要针对CTF的CRYPTO类赛题的脚本的撰写。时间仓促，难免有所纰漏。后续会不定期更新该教程。

已同步更新至Github仓库：https://github.com/tl2cents/SageMath-Concise-Tutorial。



## 格理论基础

**SageMath**（System for Algebra and Geometry Experimentation），是一个覆盖许多数学功能的应用软件，包括代数、组合数学、图论、计算数学、数论、微积分和统计。尤其是数论上的许多算法，sagemath都集成了其实现接口。sagemath本质上是以**python编程语言为基础的数学应用软件**，因此需要有一定的python基础和数论理论基础。

在介绍sagemath的使用之前，我们先简要回顾一下基本数论以及介绍格理论的相关内容。



### 基本数论

SageMath需要的基本数论理论和线性代数如下，为前置内容，此处不赘述：

- **有限域$$Zmod(N)$$**
- **有限域的阶、循环群**
- **模多项式环**
- **离散对数**
- **矩阵的秩**
- **行列式**
- **线性子空间（线性相关）**



### 格理论



#### 格的定义

算法数论中常常遇到这样一个问题：它有连续与离散两种成分，一个典型的例子就是满足某些不等式的整数系之寻求,具有这个性质的问题通常可以用格的算法理论来成功求解。标准地来说：**一个格是欧式向量空间的一个离散子群**。



回忆线性线性代数的基本内容：欧式向量空间是实数域$$\R$$上的一个有限维向量空间$$E$$，并且定义了内积。实数的n元组的全体构成了$$\R$$上一个n维空间，实数坐标空间$$\R^n$$就能认为是一个n维欧式空间。简单地说，二维欧式空间就是：
$$
E^2=\{(x,y)|x\in \R ,y\in \R\}
$$

在线性代数里实数域上对n个d维线性无关向量$$\{b_i\in \R^d,i=1,2,...,n\}$$的线性组合（**线性子空间**）可以表示为：
$$
L_R=\sum_{i=1}^n\R b_i=\left \{ \right. \sum_{i=1}^nc_ib_i:c_i\in \R,i=1,2,...,n \left\}\right.
$$
而格的定义则将系数$$c_i$$是转到了整数域$$\Z$$上：
$$
L_R=\sum_{i=1}^n\Z b_i=\left \{ \right. \sum_{i=1}^nc_ib_i:c_i\in \Z,i=1,2,...,n \left\}\right.
$$


在此基础上我们给出$$(1,0),(0,1)$$张成的线性子空间和二维格的形象化表示：

- 二维子空间，二维全平面：

![如何在WORD插入直角坐标系](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAP8AAADGCAMAAAAqo6adAAAAV1BMVEX///9eXl6bm5va2toAAADz8/Ph4eFAQECDg4Pw8PDb29ve3t7r6+vOzs5UVFSGhobAwMCSkpKzs7MqKio7OzsVFRVvb280NDRdXV0QEBC5ubmZmZmtra0mqfcXAAAB8UlEQVR4nO3Zy04iUQBFUR4XERVsfNG2/v93GqdGBlZMVlvuNSU32aeq7ojFIkmSJMlH+xtdQG0OtzqB+jPGTjdI2zGOugG6G2Pcr3WFc7m9f3i40xXObnPc7za6Qlpd6QJr9Ytv/7v26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z26wKr/brAar8usNqvC6z264JvsruYdGw2+9ePT/sJx1bX315iXJ/G4/Lcy1wv/y4/dzqc+eGnOTyPMZ7/ffWxraZdm//PxWmM49c/5tnc/6vt5c2EY7PZv5myfkb7J2q/LrDarwus9usCq/26wGq/LrDarwus9usCq/26wGq/LrDarwus9usCq/26wHr55ftf5/L/Z5IkWbwB3P0HcPNl8F0AAAAASUVORK5CYII=)



- 二维格：

![坐标变换- 知乎](data:image/jpeg;base64,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)

​		也就是上图方格中所有的格点，这也是为什么我们称这个空间为格空间，如下图

![img](https://www.ruanx.net/content/images/2021/01/SquareLattice.svg)



本教程中不涉及复杂的格理论，对于格，在CTF中最常用的就是形式是把它表示为矩阵形式，考虑一个$$\Z 上的n*n矩阵$$：
$$
M=\begin{bmatrix} 
m_{11} & m_{12} &...& m_{1n} \\ 
m_{21} & m_{22} &...& m_{2n} \\
... &... &... &...\\
m_{n1} & m_{n2} &...& m_{nn} 
\end{bmatrix}
=[m_1,m_2,...,m_n]^T\\
where \ m_{ij}\in \Z \quad and \ m_i \ is \ M's \ row \ vector \\
\\
L=Z_{span}\{m_1,m_2,...,m_n\}
$$
CTF中许多模方程的问题都可以转换到M的行向量$$[m_1,m_2,...,m_n]$$所张成的n维格空间L中进行求解。



#### 最短向量问题（Shortest Vector Problem：SVP）

格中的最短向量问题，亦即齐次逼近问题，规范式的描述是：

- **SVP问题**

  给定一个正秩格$$L$$，模长映射为$$q: L \rightarrow \R$$，寻求一个非零元素$$x\in L$$使得模长$$q(x)$$最小（尽可能地小）。

  利用上面矩阵表示的形式来阐述就是，求$$x \in L$$，模长尽可能地小，即：
  $$
  \min \Vert x \Vert^2 \quad s.t. \ x=\sum_{i=1}^n c_i* \vec m_i \quad where \ c_i \in \Z
  $$

关于格的最小向量问题的最主要的理论成果是Minkowski定理

- **Minkowski定理**

  每一个正秩n维格L皆包含一个非零元素x满足:
  $$
  q(x)\le \frac{4}{\pi}*\frac{n}{2}!^{\frac{2}{n}}*d(L)^\frac{2}{n}\le n*d(L)^\frac{2}{n}
  $$
  其中$$d(L)$$是格L对应矩阵的行列式。



然而，我们需要注意Minkowski定理是非构造性的，也就是我们知道其存在性，但是并没有有效的算法可以得到上述x。为了解决格中的最短向量问题，我们介绍一个经典算法：LLL算法。

- **LLL算法**

  在格上找到一组基，满足如下效果

  <img src="C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/lll-def.png" width = "70%" height = "50%" alt="图片名称" align=center />

  并且这组基具有如下性质：

  <img src="C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/lll-property.png" width = "70%" height = "50%" alt="图片名称" align=center />

  **LLL算法很好的一个性质是可以在多项式时间内找到符合上述条件的一组基**，并且在sagemath上有实现接口。另外一个效果可能更好、参数设置过大时间复杂度达到指数级别的SVP求解算法是**BKZ算法**，多数情况下利用LLL算法已经足够，这里不再详细介绍。



#### 最近向量问题（Closest Vector Problem：CVP）

格的最近向量问题或称非齐次逼近问题规范化定义如下：

- **CVP问题**

  在一个欧式向量空间$$E$$中给定一个格$$L$$，以及一个元素$$x\in E$$，寻求$$y\in L$$具有最小的可能距离$$d(x,y)$$。

  利用上面矩阵表示的形式来阐述就是，求$$y \in L$$，模长尽可能地小，即：
  $$
  \min \Vert y-x \Vert^2 \quad s.t.\quad y=\sum_{i=1}^n c_i* \vec m_i \quad where \ c_i \in \Z
  $$


  当然我们还可以在CVP上增加约束：

  给定一个Lattice $$L$$，与一个随机点$$t$$，还有搜索距离$$d$$，并且假设,$$\mu(t,L)\le d$$，CVP问题是让我们找到一个合理的格点,$$B*x \in L$$并且这个点到$$t$$的距离小于等于$$d$$。



简单来说就**寻找格L中距离非格点x最近的一个格点y**，我们不过多介绍理论部分，这里给出一个CVP问题求解的一个常用算法Babai's nearest plane algorithm：

- **Babai算法**

  输入秩为n的格L的一组基$$B$$和一个目标向量t，输出CVP问题的近似解

  ![img](./crypto_img/babai_1.png)

  从算法可以看出，Babai是以LLL算法为基础的，因为Baiba算法需要在性质较好的规约基上运行。



<div style="page-break-after:always"></div>

## SageMath简明教程

SageMath 是一个基于GPL协议的开源数学软件。它使用Python作为通用接口，将现有的许多开源软件包整合在一起，构建一个统一的计算平台。

对于sagemath，将它理解为python在数学应用上的一个扩展应用程序即可，在你掌握了python的基本语法后，sagemath相当于多了许多自带扩展库，熟悉这些库的基本应用，足以应对大多数CTF的密码学赛题。



sagemath学习比较大的一个问题在于缺失比较好的中文文档，许多时候我们难以找到sage中许多现成的框架，主要原因在于本地安装好sage查看源码的功能也比较麻烦，甚至不如直接去sage的Github仓库找函数。特别是目前sage的两个函数`search_doc`和`search_src`使用起来尤为不便，你可以在sage的console或者notebook输入`search_doc("matrix")`，返回元素冗长而难懂。本人尝试写该节教程，希望有助于sagemath的入门学习，囿于时间和精力限制，暂时只就sagemath的部分常用函数都进行简单介绍。



### sage安装与环境配置

- sagemath的官网下载地址：https://www.sagemath.org/，支持windows，linux，MacOS多种系统。
- 由于sage的本地环境较大（接近10G），如果不想在本地安装，sage也提供了在线的环境https://sagecell.sagemath.org/，以及cocalc：https://cocalc.com/，建议使用**cocalc**。你可以使用Github，Twitter，Google等账号注册cocalc。当然在线环境会比较慢，如果想深入学习sage建议使用本地环境。



**以windows系统下本地的sage 9.2 环境为例**：

三个常用的sage交互接口如下：

![image-20220225143015516](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225143015516.png)

三个接口常见的使用方法：

- **Notebook**

  打开后会自动在本地搭建一个Jupyter Notebook，会自动弹出浏览器访问，本地访问对应的url，Notebook的界面如下：

  ![image-20220225150417204](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225150417204.png)

  

  我们可以直接打开对应sage或者python脚本进行编辑，但是不能运行。可以new一个python3或者sage的notebook来编写、调试、运行代码:

  ![image-20220225151635031](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225151635031.png)

  

  解一个一元二次方程的代码实例，之后sage内**ctrl+c**可以退出环境：

  ![image-20220225151857248](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225151857248.png)

  

  

- **Shell**

  一个命令行工具，基本的linux的命令行指令都可以运行，可以在sage安装许多必要的python包，如安装pycryptodome:

  ![image-20220225151127130](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225151127130.png)

  

  sage中可用命令行命令`cd`任意更改当前路径，进入目标路径后通过命令`sage xxx.sage` 运行脚本

  

  另外可以在shell里面的目录下运行 `jupyter notebook` 在对应目录下打开notebook。

  更多使用帮助参考:   `sage -h`

  

- **sage  console**

  第三个sage虚拟环境应该是sage最好用的交互方式了。

  如图是sage的复数域、整数换、有理数域

  ![image-20220225154439153](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225154439153.png)

  

  可以每写完一行执行一次，也可以批量执行代码 (可以直接复制粘贴到sage) ，如下：

  ```python
  sage: from hashlib import md5
  ....: username = b'Android'
  ....: password = b'Greatly'
  ....: mid_hash = "1a9852e856816224"
  ....:
  ....: for hour in range(24):
  ....:     for minute in range(60):
  ....:         for second in range(60):
  ....:             s1 = str(hour) + str(minute) + str(second)
  ....:             s2 = md5(s1.encode()).hexdigest()[8:24]
  ....:             s = b"flag{" + s2.encode() + b"}" + username + password
  ....:             res=md5(s).hexdigest()
  ....:             if mid_hash in res:
  ....:                 print(s)
  ....:                 break
  ....:
  b'flag{80d0169d22da3c35}AndroidGreatly'
  sage:
  ```

  

  

  console是入门sage很重要的环境，调试和写代码都相对简便。在console内也可以切换目录，文件打开的相对路径都是以当前路径为准的，因此如果你需要导入sage脚本或者打开数据文件都需要确认一下文件是否存在在当前目录下：

  ![image-20220225155157572](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225155157572.png)

  

  最重要的一个调试方式是，**tab键可以自动补全**，如下图，我们想要知道多项式f的根方法，tab键后即可找到**roots**：

  ![image-20220225155831597](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220225155831597.png)

  

  另外再介绍一个sage比较好用的源码查看的方法，比如我们想查看sage的离散对数函数的实现代码，console里或则notebook内输入`discrete_log??`，得到函数的具体用法和源码：

  

   ```python
  Signature:
  discrete_log(
      a,
      base,
      ord=None,
      bounds=None,
      operation='*',
      identity=None,
      inverse=None,
      op=None,
  )
  ...
   ```

  

  

  下翻可查看源码，按 q  退出查看：

  ![image-20220228163656344](C:/Users/tang2000/Desktop/SomethingForNothing/CTF/信安实践二级教材/cryptoⅡ/crypto_img/image-20220228163656344.png)

  

  如果你不需要查看源码，只需要：`discrete_log?`即可。同理对于类的方法实现代码同样可以按照类似方法查看，如：`f.small_roots??`。





### sage基本函数使用

基本的python方法这里就不再赘述，这里主要讲sage**自带的一些函数与特殊运算**，由于目前sagemath的中文相关文档较少，这里详细介绍一下CTF中经常用到的函数。



#### sage与python的不同

```python
5       # 5的类型是整数环上的元素，而不是int
sage: type(5)
<class 'sage.rings.integer.Integer'>

# 注意int(5)和5的方法是不一样的，如5的比特长方法
sage: int(5).bit_length()
3
sage: 5.nbits()
3

5**3	#  **代表幂
5^3     #  ^也是幂，python中是异或
5^^3    #  ^^表示sage中的异或
10/3    # 注意10/3是一个有理数，而在python中它会变成小数
sage: type(10/3)
<class 'sage.rings.rational.Rational'>
```



另外在编程中特别需要注意的一点是$$pow(a,x,N)$$运算，它的返回结果是Zmod(N)上的元素，你之后不在在这个有限环上运算，最好转成正常整数$$ZZ(pow(a,x,N))$$再运算:

```python
sage: type(pow(2,4,7))
<class 'sage.rings.finite_rings.integer_mod.IntegerMod_int'>

sage: type(ZZ(pow(2,4,7)))
<class 'sage.rings.integer.Integer'>
```





#### 基本的环、域

- **整数环** ：ZZ，也等价于Integers

- **有理数环**： QQ

- **实数域**：RR

- **复数域**：CC

- **多项式环：PolynomialRing()**

  ```python
  # 定义在有理数环上的多项式环
  sage: PQ.<x> = PolynomialRing(QQ)
  ```

  PQ：**多项式环**的名字，自定义

  x：变量的名字，自定义

- **其他有限域、环**：

​		**伽罗瓦域**(N必须是某个素数或者某个素数的k次方)：GF(N)、GF(2^8,modulus=[1,0,0,1,1,1,0,0,1])

​		**一般有限环**：Zmod(N)



一个元素在各种域上的转换示例如下：

```python
sage: e=12
sage: CC(e)
12.0000000000000
sage: ZZ(e)
12
sage: G=GF(7)
sage: G(e)
5
sage: Z7=Zmod(7)
sage: Z7(e)
5
```



#### **基本数论函数**

- **求逆**：$$e模n的逆$$

  定义在Zmod(N)上的元素e，直接`e^-1`或者`1/e`

  否则直接使用`inverse_mod(e,n)`函数获得e模n的逆

  

- **最大公因数**：（a,b)的最大公因数

  `gcd(a,b)`

  

- **最大公倍数**：(a,b)的最大公倍数

  `lcm(a,b)`

  

- **模幂**：$$e^x \mod n$$

  定义在Zmod(N)上的元素e，直接`e^x`

  否则直接使用`pow(e,x,n)`

  

- **素数判断**

  ```python
  sage: x=123721984710347123123
  sage: x.is_prime()
  False
  ```

  

- **阶乘**：

  `factorial(x)`

  

- **欧拉函数**：

  ```python
  euler_phi(n) 							
  #  求n的欧拉函数 phi = n*prod([1 - 1/p for p in prime divisors(n)])
  ```

  

- **中国剩余定理求解**
  $$
  x \equiv m_1 \mod n_1 \\
  x \equiv m_2 \mod n_2 
  $$
  解为：

  ```python
  crt([m1,m2],[n1,n2])
  sage: crt([1,2],[3,5])
  7
  ```

  

- **扩展欧几里得算法**：
  $$
  d=gcd(a,b)=u*a+v*b
  $$


  ```python
  d,u,v=xgcd(a,b)
  ```

  

- **素数分解**

  ```python
  factor(1024) -> 2^10
  prime_divisors(1024) -> [2]
  divisors() #所有因子
  ```

  

- **开根(有限域开根)**

  整数域开根
  $$
  x^m=y\\
  已知m，y求x
  $$

  ```python
  sage: y=87^8
  sage: y.nth_root(8)
  87
  ```

  有限域开根：
  $$
  x^m \equiv y\mod N\\
  已知m，y，N求x
  $$

  ```python
  sage: y=pow(78,888,65537)
  # 此时已经定义在有限域Zmod(65537)上
  sage: y.nth_root(888)
  65459
  sage: x=y.nth_root(888)
  sage: x+78
  0
  ```

  注意：**开根有多解，nth_root只会返回一个解**，如果需要得到所有解，可以加一个参数：

  ```python
  sage: x=y.nth_root(888,all=True)
  ```

  当然，sage的有限域开根函数实现并不好，可以手动实现`AMM`算法或者使用更为强大的工具`mma`（`mma`解方程的性能绝对是世界顶尖水平的）。

  

- **离散对数**

  sage上实现了多种离散对数的求解方式，包括gp接口的离散对数求解。

  参数说明：求解以`base`为底，`a`的对数；`ord`为`base`的阶，可以缺省，`operation`可以是`+`与`*`，默认为`*`；`bounds`是一个区间`(ld,ud)`，需要保证所计算的对数在此区间内。

  - `discrete_log(a,base,ord,operation)`

    通用的求离散对数的方法。

  - `discrete_log_rho(a,base,ord,operation)`

    求离散对数的Pollard-Rho算法。

  - `discrete_log_lambda(a,base,bounds,operation)`

    求离散对数的Pollard-kangaroo算法（也称为lambda算法）。

  - `bsgs(base,a,bounds,operation)`

    小步大步法。

    

  其中operation为+时一般是应用在椭圆曲线（ECC）的离散对数，ECC部分本教程暂未涉及。

  代码示例：

  ```python
  #生成32位的素数p作为模数，int为32位，超过int要在数字后加L
  p=random_prime(2L**32)
   
  #定义有限域GF(p)
  G=GF(p)
   
  #找一个模p的原根
  gp ('znprimroot('+str(p)+')')
  #输出Mod(rt,p)，则x是模p的原根
  g=G.primitive_element()
  
  #生成私钥
  x=G(ZZ.random_element(p-1))
   
  #公钥y=g^x mod p，由于已经定义在GF(p)上，因此g**x就是g^x mod p
  y=g**x
   
  #计算离散对数的通用方法
  discrete_log(y,g)==x
  #计算离散对数的lambda方法
  discrete_log_lambda(y,g,(floor(ZZ(x)/2),2*ZZ(x)))==x
  #小步大步法计算离散对数
  bsgs(g,y,(floor(ZZ(x)/2),2*ZZ(x)))==x
  
  #n为质数或质数幂（线性筛Index Calculus）
  R = Integers(99)
  a = R(4)
  b = a^9
  b.log(a)
  n=99
  #或
  x = int(pari(f"znlog({int(b)},Mod({int(a)},{int(n)}))"))
  x = gp.znlog(b, gp.Mod(a, n))
  # 代码修改自 Lazzaro @ https://lazzzaro.github.io
  ```

  输出：

  ```python
  Mod(2, 3223324843)
  True
  True
  True
  9
  ```



#### 线性代数相关函数

- **矩阵定义、运算**

  **一般矩阵定义**：

  ```python
  # 直接定义ZZ矩阵并且初始化
  A = Matrix(ZZ,[[1,2,3],[3,2,1],[1,1,1]]) 
  A = matrix(Zmod(2),[[1,2,3],[3,2,1],[1,1,1]]) 
  # 模2有限域上的矩阵
  B=matrix(GF(2),A)
  # 向量定义
  Y = vector(ZZ,[0,-4,-1]) 
  Y = vector(GF(2),[0,-4,-1]) 
  
  # 定义矩阵但不初始化：GF（2）上的10*10矩阵，默认所有元素为0
  m = matrix(GF(2), 10, 10)
  ```

  

  **分块矩阵定义**：
  $$
  M=\begin{bmatrix} 
  A & B \\ 
  C & D 
  \end{bmatrix}
  $$

  ```python
  A = matrix(GF(2), 10, 10)
  B = matrix(GF(2), 10, 30)
  C = matrix(GF(2), 5, 10)
  D = matrix(GF(2), 5, 30)
  M=block_matrix([[A,B],[C,D]])
  #等价于
  M=block_matrix([[A,B],[C,D]],nrows=2)    
  ```

  

  **矩阵运算**：

  ```python
  m1=matrix([[1,2], [1,3]])
  m2=matrix([[2,1], [2,3]])
  m1+m2
  m1*m2
  输出
  [3 3]
  [3 6]
  [ 6  7]
  [ 8 10]
  ```

  转置：`M.transpose()`

  求逆： `M.inverse()`或者$$M^{-1}$$

  特征值：`M.eigenvalues()`

  特征向量（右）：`M.eigenvectors_right()`

  行列式： `M.det()`

  求秩：`M.rank()`

  

- **解线性方程组**：

  对于任意线性（模）方程组，sage都可以很方便地求解：
  $$
  线性方程组：
  \left\{\begin{align}
    a_{11}*x_1+a_{12}*x_2+...+a_{1m}*x_m=b_1\\
    a_{21}*x_1+a_{22}*x_2+...+a_{2m}*x_m=b_2\\
    ...\\
    a_{n1}*x_1+a_{n2}*x_2+...+a_{nm}*x_m=b_n\\
  \end{align}\right.\\
  \\
  矩阵表示：
  A*X=B\\
  其中A\in \R^{n*m} , X \in \R^m,B\in \R^n
  $$
  其中$$\R$$可以替换成任意域：$$Zmod(N)、ZZ、QQ$$

  给定有限域或者整数域上的矩阵$$A,B$$，求解$$A*X=B$$：

  ```python
  X = A.solve_right(B)
  #等价于
  X = A \ B
  ```

  给定有限域或者整数域上的矩阵$$A,B$$，求解$$X*A=B$$：

  ```python
  X = A.solve_left(B)
  ```

  特别地，sagemath还集成右（左）核空间的求解，也就是$$A*X=0和X*A=0$$的所有解空间

  ```python
  X = A.left_kernel()  # X*A=0
  X = A.right_kernel() # A*X=0
  # 矩阵形式
  X=A.right_kernel_matrix()
  ```

  

  对于**一般（模）方程的求解：**

  ```python
  # 一般方程
  x, y = var('x, y')
  solve([x+y==10, x-y==0], x, y)
  
  # 解模方程
  sage: solve_mod([x+y==66, x-y==23],97)
  [(93, 70)]
  ```



#### 多项式环及其函数

- **多项式环定义**

  前面已经稍微涉及了多项式环的定义：

  ```python
  # 定义在有理数环上的一元多项式环
  sage: PQ.<x> = PolynomialRing(QQ)
  # PQ：多项式环的名字，自定义
  # x：变量的名字，自定义
  
  # 或者等价定义
  sage: PQ = PolynomialRing(QQ,'x')
  sage: x=PQ.gen()
  # 或者
  sage: PQ = QQ[‘t’]
  ```

  二元多项式环可以如下定义，其他类似：

  ```python
  PQ2.<x,y> = PolynomialRing(QQ)
  ```

  

- **多项式定义**

  在定义好多项式环后：`PQ.<x> = PolynomialRing(QQ)`

  ```python
  PQ.<x> = PolynomialRing(QQ)
  f=4*x^3+2*x+1
  ```

  

  一个等价的定义方式是用列表：

  ```python
  sage: PQ.<x> = PolynomialRing(QQ)
  ....: PQ([1,2,0,4])
  4*x^3 + 2*x + 1
  ```

  

  生成随机多项式

  ```python
  sage: R.<y> = PolynomialRing(GF(65537))
  ....: degree=7
  ....: S = R.random_element(degree)
  sage: S
  60064*y^7 + 31544*y^6 + 19047*y^5 + 19873*y^4 + 53675*y^3 + 39961*y^2 + 40289*y + 30493
  ```

  

  特别地，对于伽罗瓦域GF(2^n)上元素的声明，如下（**注意模多项式需要是本元多项式**）：

  ```python
  F.<x> = GF(2^8,modulus=[1,0,0,1,1,1,0,0,1])
  F.fetch_int(21) == F(21.bits())
  F(21.bits()).integer_representation()#逆过程
  ```

  

- **多项式相关运算、函数**

  多项式分解：

  ```python
  sage: R.<y> = PolynomialRing(GF(65537))
  sage: f=R.random_element(degree)*R.random_element(degree)
  sage: f.factor()
  (64343) * (y + 10474) * (y + 49729) * (y^2 + 7588*y + 41809) * (y^3 + 20885*y^2 + 60459*y + 29275) * (y^3 + 27396*y^2 + 16874*y + 56765) * (y^4 + 25258*y^3 + 12887*y^2 + 59574*y + 64190)
  ```

  

  多项式最大公因式：

  ```python
  sage: g=R.random_element(degree)
  sage: f1=R.random_element(degree)*g
  sage: f2=R.random_element(degree)*g
  sage: gcd(f1,f2)
  y^7 + 10115*y^6 + 11715*y^5 + 59953*y^4 + 53514*y^3 + 2238*y^2 + 55526*y + 46381
  sage: f1.gcd(f2)
  y^7 + 10115*y^6 + 11715*y^5 + 59953*y^4 + 53514*y^3 + 2238*y^2 + 55526*y + 46381
  ```

  

  首一多项式：

  ```python
  sage: f.monic()
  y^14 + 10256*y^13 + 12184*y^12 + 38005*y^11 + 59412*y^10 + 48187*y^9 + 45070*y^8 + 34871*y^7 + 50384*y^6 + 61263*y^5 + 52827*y^4 + 24231*y^3 + 4268*y^2 + 31669*y + 46586
  ```

  

  多项式的根：

  ```python
  sage: f.roots()
  [(55063, 1), (15808, 1)]
  
  # small roots,使用前必须保证f是首一的多项式
  # 关于small_roots的详细参数可以参考coppersmith节
  sage: f.monic().small_roots()
  []
  ```

  

  多项式的结式 ：

  ​	在两个多项式有公共根时,可以求结式，之后再求解根

  ```python
  sage: f.resultant(g)
  51209
  ```

  

  Groebner basis：

  在已知模方程的一些关系后，我们可以通过Groebner basis得到一些方程的根，如下图得到的就是$$a,b,c$$三个解

  ```python
  sage: G=GF(next_prime(getrandbits(512)))
  sage: a=G(getrandbits(512))
  sage: b=G(getrandbits(512))
  sage: c=a+b+233
  sage: a3=a^3
  sage: b3=b^3
  sage: c3=c^3
  sage: x,y,z =G['x,y,z'].gens()
  ....: I = ideal(z-x-y-233,  x^3-a3 ,y^3-b3 , z^3 - c3)
  ....: B = I.groebner_basis()
  verbose 0 (3837: multi_polynomial_ideal.py, groebner_basis) Warning: falling back to very slow toy implementation.
  sage: B
  [x + 4115365407930271672145917959064197986875879923337260019480399129522546957414196891761965008866716311717547095730704169414503027605502719768955802977966739, y + 1438196640478633988394708656097782124050629313931426401244798821904034601463695109291179031271158354211457757606425866100114378067643728009244830918500697, z + 5553562048408905660540626615161980110926509237268686420725197951426581558877892001053144040137874665929004853337130035514617405673146447778200633896467203]
  ```



#### 格相关函数*

sagemath集成了格上许多经典算法，在这里介绍crypto中常用的几个：

- **LLL算法**

  ```python
  sage: sage: M=matrix.random(GF(next_prime(getrandbits(10))),5,5)
  ....: sage: M=M.change_ring(ZZ)
  ....: sage: M.LLL()
  [-62  55 -57  -9 -48]
  [-59  58  74  22 -39]
  [ 11   7 -39   8 125]
  [144  60 -14 -41  24]
  [ 41  25  17 176 -11]
  ```

  

- **coppersmith算法**

  也就是前文提到的small_roots函数，其中对多项式有如下约束：必须是首一多项式（monicpolynomial），可以使用$$f=f.monic()$$解决；f必须是一元多项式，暂不支持多元多项式。

  ```python
  # 这些参数的调整可以参加coppersmith节
  x=f.small_roots(X=upperbound,beta=0.5,epsilon=1/20)
  ```

  

- **Babai算法**

  Babai算法sagemath上并没有集成的接口，这里给出一个CTF中crypto常用的实现：

  ```python
  # 输入矩阵 basis，目标向量，v
  def approximate_closest_vector(basis, v):
      """Returns an approximate CVP solution using Babai's nearest plane algorithm.
      """
      BL = basis.LLL()
      # 施密特正交化基
      G, _ = BL.gram_schmidt()
      _, n = BL.dimensions()
      small = vector(ZZ, v)
      for i in reversed(range(n)):
          c = QQ(small * G[i]) / QQ(G[i] * G[i])
          c = c.round()
          small -= BL[i] * c
      return (v - small).coefficients()
  ```

