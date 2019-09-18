---
title: machine learning in finance
date: 2019-08-02 14:31:09
tags:
categories:
top:
mathjax: true
---
**沪深300指数**由上海和深圳证券市场中市值大、流动性好的300只股票组成，综合反映中国A股市场上市股票价格的整体表现。
**恒生指数**(Hang Seng Index)是以反映香港股市行情的重要指标，指数由五十只恒指成份股的市值计算出来的，代表了香港交易所所有上市公司的十二个月平均市值涵盖率的63%。
**标普500**是由标准普尔於1957年创立的，被广泛认为是三只股指中衡量美国经济状况最好的一项指标。标普500股票平均价格指数是由每支成分股公司的市场价值之和除以一个由标准普尔设定的除数得到的终值。简而言之就是，所有股票的市值总和除以标普除数，或总市值/标普除数。
**道琼斯工业平均指数**，简称道指，是三只股指中历史最为悠久也是全球最知名的股指。道指最早是在1884年由道琼斯公司的创始人查尔斯·亨利·道开始编制的一种算术平均股价指数。道指代表了华尔街日报确认的30只大盘股。不同於标普500和纳指，道指成分股的比重是根据股票价格排序的，也就是说股票价格越高的公司越能影响道指的表现。道指股票价格平均指数是入选股票的价格之和除以道指除数得到的终值。
**纳斯达克指数**，简称纳指，1985年开始交易，是三只股指中最为年轻的股指。纳指代表著在纳斯达克上市的最大的非金融公司，其中科技股所占比重较大，因此通常被认为是一只科技股指。该股指是根据每个公司的市场价值来设置权重，这意味著每个公司对指数的影响力是由其市场价值决定的。

**市场（或代表性指数）收益率market rate of return**：国内市场通常使用沪深300指数作为市场代表，美股则常用标普500指数，港股则常用恒生指数

**无风险利率risk-free rate**：这是投资者将资金投资于某一项没有任何风险的投资对象而能得到的利息率，例如以人民币投资的中国国债、美元投资的美国国库券、以欧元交易和投资的德国政府债券等。该数字通常以百分比表示。
![Investing.com - 中国政府债券](2019-08-12 11 34 20.png)

# Capital Asset Pricing Model资本资产定价模型
在金融世界，人们手持多余的资金时候本能欲望都会寻求让资金尽量增长，投资因此诞生。但并不是每项投资都能带来收益，人的本能让人追求收益畏惧风险。
在现实世界的自由投资途径中，低风险高收益的投资项人们会蜂拥而至进行瓜分，高风险低收益的对象会逐渐被人抛弃。
现实世界的投资途径如此之多，有一类投资能让你保证获得收益，这类投资称为无风险收益，例如以国家信用做背书的国债和银行定期存款。但有些人为了获得更高的收益愿意承担一定的风险选择投资股市，因此有一个市场的平均收益。有没有一个收益最大风险最小的投资组合？它在哪里？这是每一个投资者每天都在考虑的问题。为了搞清这个问题，经济学家尝试简化世界，构建了CAPM模型来进行风险评估和收益预计：
$$
E(r_{i})=r_{f}+\beta[E(r_{m})-r_{f}]
$$
其中
- $E(r_{i})$是资产$i$的期望收益率;
- $r_{f}$是无风险收益率，通常以短期国债的利率来近似替代;
- $\beta$是资产$i$的系统性风险系数;
- $E(r_{m})$是市场收益率，通常用股票价格指数收益率的平均值或所有股票的平均收益率来代替;
<!-- - $E(r_{m})-r_{f}$是市场风险溢价(Market Risk Premium)，即市场投资组合的期望收益率与无风险收益率之差。 -->
这个简洁的模型将无风险收益与市场平均收益都考虑进来，毕竟如果没有一个预估的高收益吸引投资者，那还不如把钱存个定期。

# Beta系数
**Beta系数$\beta$**作为一个统计学上的概念，用以度量一项资产系统性风险的指标，是资本资产定价模型的参数之一。指用以衡是资产i的系统性风险系数，量一种证券或一个投资证券组合相对总体市场的波动性的一种证券系统性风险的评估工具。Beta系数可用于计算股票的预期收益率。Beta系数是股票分析师们在选择资产组合中的股票时要考虑的基本因素之一，其他因素包括市盈率、股东权益、资本负债比率等。
- 如果是负值，则显示其变化的方向与大盘的变化方向相反;大盘涨的时候它跌，大盘跌的时候它涨；有一些行业组织，如黄金矿工，其中负beta很常见。
- $\beta$ =0，表示投资组合和市场走向没有相关性，如固定收益类；
- 当$\beta$=1，说明投资对象的价值与市场强烈相关。在投资组合中加入Beta为1的股票并没有增加任何风险，也没有增加获得超额收益的可能性。
- 当$\beta$<1，显示其价值波动理论上小于市场，意味着在投资组合中加入这样的对象能减小风险，公共事业股票utility stocks通常有较低的beta，因为它们变化往往比市场平均值更慢。
- 当$\beta$>1，显示其价值波动理论上大于市场。例如如果一个股票的Beta为1.2，说明其比市场还多20%的波动性，科技股通常拥有比市场基准更高的beta，这样的对象会增加投资组合的风险但也更可能带来超额收益。


## Beta系数有什么用;

- 使用Beta来选择股票是减少波动性和创建更多元化投资组合的工具之一。
- 计算资本成本，进行资产估值、做出投资决策、制定考核及激励标准
- 计算单个资产或组合的系统风险(投资组合的Beta等于单个Beta系数的加权求和)，进行投资管理
- 牛市时选择高Beta证券，将成倍放大市场收益；熊市时选择低Beta证券以抵御市场风险

<!-- ![](v4-728px-Calculate-Beta-Step-3-Version-4.jpg.jfif) -->
![图中我们可以看出，谷歌股票的回报波动比较大（蓝色），标普500的回报波动比较小（橙色）](download.png)

# Beta系数计算方法

## 通过相关系数计算Beta
既然Beta是衡量股票收益相对于市场收益的风险程度，于是可以通过计算两者的相关系数来确定Beta：

$$
Beta=\beta_{p}=\frac{Cov(D_{p},D_{m})}{Var(D_{m})}
$$
其中$$D_{p}$$为策略每日收益，$$D_{m}$$为大盘每日收益，$$Cov(D_{p},D_{m})$$是策略每日收益与大盘每日收益的协方差,$$Var(D_{m})$$为大盘每日收益方差

## 通过历史已知的策略收益反推
在CAPM模型中如果已知历史策略收益，那么带入已知项可求得Beta：
$$
Beta=\beta_{p}=\frac{E(r_{i})-r_{f}}{E(r_{m})-r_{f}}
$$

## 通过线性回归拟合
既然在CAPM模型中Beta作为市场收益减去无风险收益的系数，对于历史已知的股票收益，
$$
R_{p}=\alpha+\beta_{p} R_{m}+\varepsilon
$$
将大盘收益率作为Y，将策略历史收益率作为X进行回归，$\varepsilon$是建模误差。通过最小化回归方程预测的 y 值与实际 y 值之间的平方差（垂直距离），能够得到一条最合理的回归直线，直线与Y轴截距是事后的 $\alpha$，即与市场指数回报相比，超额回报的衡量。如果截距是负数，则意味着策略在风险调整的基础上表现落后于大盘，而截距是正数，则意味着其在风险调整基础上有超额收益。回归线的斜率系数 Beta 被计算为 x 和 y 的协方差除以 x 的方差，在数学上与方式一是一致的。

这种方式得出的Beta值相对稳定
![](download1.png)

# Alpha系数
实际风险回报和平均预期风险回报的差额即 α 系数。
投资中面临着系统性风险(即$\beta$)和非系统性风险(即$\alpha$)，$\alpha$是投资者获得与市场波动无关的回报。比如
- α>0，表示一基金或股票的价格可能被低估，建议买入。亦即表示该基金或股票以投资技术获得比平均预期回报大的实际回报。较高的α一般由股票的个性特征所决定，与大势和行业无关，应尽可能寻找高α的个股。
- α<0，表示一基金或股票的价格可能被高估，建议卖空。亦即表示该基金或股票以投资技术获得比平均预期回报小的实际回报。
- α=0，表示一基金或股票的价格准确反映其内在价值，未被高估也未被低估。亦即表示该基金或股票以投资技术获得平均与预期回报相等的实际回报。

# Alpha的计算
alpha是超额收益，它与市场波动无关，也就是说不是靠系统性的上涨而获得收益。
$$
Alpha=\alpha=\mathrm{R}_{p}-\left[R_{f}+\beta *\left(R_{m}-R_{f}\right)\right]
$$

# 使用Python计算Beta及Alpha系数



## CAPM及Beta、Alpha系数的局限
Beta系数为股票评估提供了参考，但它具有它固有的局限性。尽管Beta可用于确定证券的短期风险，并用于CAPM分析波动率以计算股权成本，由于β系数是使用历史数据点计算的，因此对于希望预测股票未来走势的投资者来说，它变得没那么有意义。Beta对长期投资的作用也不大，因为股票的波动性每年都会发生显着变化，具体取决于公司的增长阶段和其他因素。

CAPM(资产资本定价模型)是在金融经济学中发挥了广泛的应用，但其作为对理想情况的一个建模，建立在假设投资者信息对等、投资行为不会对股票价格产生影响的前提下。有研究学者指出，CAPM不适用于中国股票市场，收益率和Beta之间并不是线性相关。

许多学者提出异议，认为市场过于“有效”，投资者除非碰巧，否则无法重复地赚得超额收益。另一方面，由Russ Wermers领衔的对共同基金的实证研究得到的结论认为，基金经理寻找、挑选有正α值的证券是有价值的。然而这一研究结论遭到非议，批评者认为Russ的结论受到“幸存者偏差”的影响。虽然受到争议，詹森阿尔法仍然被广泛的用于评价基金经理表现。


**Good References:**
 1. [贝塔值（beta）的线性回归方法实践](https://www.cnvar.cn/2019/01/11/beta-calculation-linear-regression/)
 2. [如何计算股票的 Beta 系数](https://zh.wikihow.com/%E8%AE%A1%E7%AE%97%E8%82%A1%E7%A5%A8%E7%9A%84-Beta-%E7%B3%BB%E6%95%B0)
 3. [Stock Beta and Volatility](https://www.money-zine.com/investing/stocks/stock-beta-and-volatility/)
 4. [贝塔系数(Betas)能否为负数?一个经常被问到的面试问题](https://www.cnvar.cn/2017/08/28/negative-betas/)
 5. [沪深300指数与标普500指数差距有多大？](https://xueqiu.com/2551686004/129137796)
 6. [跨越半个世纪的资产配置量化研究，少踩坑，多赚钱](https://xueqiu.com/2551686004/102206316)