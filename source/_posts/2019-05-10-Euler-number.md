---
title: 自然常数e到底自然在哪？
date: 2019-05-10 10:42:15
tags: 自然常数
categories: 数学
top:
mathjax: true
---
自然常数$e$是一个奇妙的数字，这里的$e$并不仅仅代表一个字母，它还是一个数学中的无理常数，约等于2.718281828459。
但你是否有想过，它到底怎么来的呢？为啥一个无理数却被人们称之为“自然常数”？
{% asset_img 640.webp 200 200 %}


<!-- more -->

说到$e$，我们会很自然地想起另一个无理常数$\pi$。$\pi$的含义可以通过下图中的内接与外切多边形的边长逼近来很形象的理解。
![图片来源: betterexplained](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/641.jfif)

假设一个圆的直径为1，其外切与内接多边形的周长可以构成$\pi$ 的估计值的取值范围上下限，内接与外切多边形的边越多，取值范围就越窄，只要边数足够多，取值范围上下限就可以越来越逼近圆周率$\pi$ 。

如果说$\pi$ 的计算很直观，那$e$呢？所以在此也用一种图解法来直观理解e。

首先，我们需要知道e 这个表示自然底数的符号是由瑞士数学和物理学家Leonhard Euler(莱昂纳德·欧拉)命名的，取的正是Euler的首字母“e ”。

![Leonhard Euler (1707-1783)](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/640.jpg)
但实际上，第一个发现这个常数的，并非欧拉本人，而是雅可比·伯努利（Jacob Bernoulli）。

![伯努利家族](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/641.jpg)

伯努利家族是17〜18世纪瑞士的一个赫赫有名的家族，其中出了很多著名的数理科学家，雅可比·伯努利是约翰·伯努利（Johann Bernoulli）的哥哥，而约翰·伯努利则是欧拉的数学老师。总之，大佬们之间有着千丝万缕的联系。
{% asset_img 642.webp 150 400 %}

要了解e 的由来，一个最直观的方法是引入一个经济学名称“<span style="color:red">**复利(Compound Interest)**</span>”。

> **复利率法**（英文：compound interest），是一种计算利息的方法。按照这种方法，利息除了会根据本金计算外，新得到的利息同样可以生息，因此俗称“利滚利”、“驴打滚”或“利叠利”。只要计算利息的周期越密，财富增长越快，而随着年期越长，复利效应亦会越为明显。—— 维基百科

在引入“**复利模型**”之前，先试着看看更基本的 “指数增长模型”。


我们知道，大部分细菌是通过二分裂进行繁殖的，假设某种细菌1天会分裂一次，也就是一个增长周期为1天，如下图，这意味着：**每一天，细菌的总数量都是前一天的两倍**。
![(图片来源: betterexplained)](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510111844.png)

显然，如果经过$x$天（或者说，经过个增长周期）的分裂，就相当于翻了$x$倍。在第$x$天时，细菌总数将是初始数量的$2x$倍。如果细菌的初始数量为1，那么$x$天后的细菌数量即为$2x$：
$$
\mathbf{细菌数量}=2^{x}
$$
如果假设初始数量为$K$，那么$x$天后的细菌数量则为$K\cdot 2x$：
$$
\mathbf{细菌数量}=K\cdot 2^{x}
$$
因此，只要保证所有细菌一天分裂一次，不管初始数量是多少，最终数量都将是初始数量的2x 倍。因此也可以写为：
$$
Q=2^{x}
$$

上式含义是：**第$x$天时，细菌总数量是细菌初始数量的$Q$倍**。

如果将 “**分裂**”或“**翻倍**”换一种更文艺的说法，也可以说是：“**增长率为100%**”。那我们可以将上式写为：
$$
Q=(1+100 \%)^{x}
$$
当增长率不是100%，而是50%、25%之类的时候，则只需要将上式的100%换成想要的增长率即可。这样就可以得到更加普适的公式：
$$
Q=(1+r)^{x}
$$
**这个公式的数学内涵是**：<span style="color:red">**一个增长周期内的增长率为$r$，在增长了$x$个周期之后，总数量将为初始数量的$Q$倍**</span>。

以上为指数增长的简单实例，下面来看看雅可比·伯努利的发现：

假设你有1元钱存在银行里，此时发生了严重的通货膨胀，银行的利率飙到了100%（夸张一下，为了方便计算）。如果银行一年付一次利息，自然在一年后你可以拿到1元的本金（蓝色圆）和1元的利息（绿色圆），总共两元的余额。
![(图片来源: betterexplained)](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112331.png)
现在银行的年利率不变，但银行为了招揽客户，推出一项惠民政策，每半年就付一次利息。那么到第六个月的时候，你就能够提前从银行拿到0.5元的利息了。
![(图片来源: betterexplained)](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112343.png)

机智的你会马上把这0.5元的利息再次存入银行，这0.5元的利息也将在下一结算周期产生利息(红色圆)，专业术语叫“复利”，那么年底的存款余额将等于2.25元。
![(图片来源: betterexplained)](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112351.png)

此时，我们可以换个角度这样看：即，每个结算（增长）周期为半年，每半年的利率是50%（或者说100%/2），一年结算两次利息，且第一次结算完后，立马将利息存入。此时我们的计算公式和结果如下：
$$
Q=\left(1+\frac{100 \%}{2}\right)^{2}=2.25
$$
继续，假设现在银行为了和其他银行抢生意，短期不想赚钱了，每四个月就付一次利息！而机智的你依然一拿到利息就立马存入，与半年结算一次利息类似：即，每个结算周期为四个月，每四个月的利率是33.33%（或者说100%/3），一年结算三次利息，且前两次结算完后，都立马将所有利息存入。
![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112417.png)

此时计算公式和结果如下：
$$
Q=\left(1+\frac{100 \%}{3}\right)^{3} \approx 2.37037
$$

我的天，年利率虽然没有变，但随着每年利息交付次数的增加，你年底能从银行拿到的钱居然也在增加！

那么是不是会一直增大到无穷大呢？想得倒美…

现在假设存款人和银行都疯了，银行在保证年利率为100%的前提下连续不断地付给存款人利息，存款人天天呆在银行不走，拿到利息就往银行里存。这样，所得利息即所谓“<span style="color:red">**连续复利**</span>”。

但是，你会发现，似乎有一个“天花板”挡住了你企图靠1块钱疯狂赚取1个亿的小目标，这个“天花板”就是$e$ ！
![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112410.jpg)

如果，我们进行一系列的迭代运算，我们将看到以下结果：
![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112404.png)
其中，$n$指的是一年中结算利息的次数。

只要在年利率保持100%不变的情况下，不断地提高利息的结算次数，余额就将会逼近$e$=2.718281845…

然后，终于可以祭出这个高等数学微积分里计算$e$的一个重要极限了：
$$
e = \mathop {\lim }\limits_{n \to \infty } \left( {1 + \frac{1}{n}} \right)^n
$$
现在再回头看这个重要极限，想必会有更加直观的理解。

也就是说，就算银行的年利率是100%，再怎么求银行给你“复利”，年底也不可能得到超过本金e 倍的余额。况且，我是没见过哪个银行的年利率是100%。

虽然正常的银行不会推出连续复利这种优惠政策，但在自然界中，大多数事物都处在一种“无意识的连续增长”状态中。对于一个连续增长的事物，如果单位时间的增长率为100%，那么经过一个单位时间后，其将变成原来的$e$倍。生物的生长与繁殖，就也类似于“利滚利”的过程。

再比如，在等角螺线中：
![等角螺线](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/20190510112358.png)


如果用极坐标表示，其通用数学表达式为：
$$
r=a \cdot e^{b \cdot \theta}
$$
其中，$a$、$b$ 为系数，$r$ 螺线上的点到坐标原点的距离，$θ$ 为转角。这正是一个以自然常数$e$为底的指数函数。

例如，鹦鹉螺外壳切面就呈现优美的等角螺线：
{% asset_img 20190510112424.jpg 300 400 鹦鹉螺外壳 %}
热带低气压的外观也像等角螺线：
{% asset_img 20190510112430.jpg 300 400 热带低气压 %}
就连旋涡星系的旋臂都像等角螺线：
{% asset_img 20190510112436.jpg 300 400 旋涡星系 %}

或许这也是e 被称为“<span style="color:red">**自然常数**</span>”的原因吧。当然，自然常数e 的奇妙之处还远不止这些，一本书都写不完。

**Good References:**
 1. [自然常数e到底自然在哪？|科研狗](https://mp.weixin.qq.com/s/yAZiYYJBUJuesBCTUL_tBg)
 2. [An Intuitive Guide To Exponential Functions & e](https://betterexplained.com/articles/an-intuitive-guide-to-exponential-functions-e/)
 3. [Prehistoric Calculus: Discovering Pi](https://betterexplained.com/articles/prehistoric-calculus-discovering-pi/)
 4. [Compound interest](https://en.wikipedia.org/wiki/Compound_interest)
 5. [Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler)
 6. [Logarithmic spiral](https://en.wikipedia.org/wiki/Logarithmic_spiral)
