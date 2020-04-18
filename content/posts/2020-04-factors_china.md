---
title: 中国股市的size, value, momentum, reversal, liquidity, ivol因子
date: "2020-03-18"
categories: ["stock factors"]
description: "size, value, momentum, reversal, liquidity, ivol in China"
images: ["/images/china_factors/china_factors.png"]
---
几个常见的股票市场因子：size, value, momentum, reversal, liquidity, idiosyncratic volatility (ivol) 的中国市场简单实证. size, reversal, liquidity, ivol 比较有趣. 中国股市能否做价值投资? 
<!--more-->
**结论：**
- size, reversal, liquidity, ivol 比较有趣. 
  - 在2013年到2016年这段时间里买入size较小的股票, 收益会很好. 2017年之后小size的股票表现很差. 
  - liquidity差的股票涨的很疯. 
  - ivol小的股票涨的很稳.
  - 没发现momentum, 倒是有各种reversal.
- value? 中国股市能否做价值投资? 不知道, 至少从BM指标上看是很差的. Liu et al. (2019)[^1]说用Earnings/Price，未来有时间进一步检验一下.

## 背景
这学期接了学院里从来没有人上过, 但教学计划里又一直存在的一门硕士课——《量化投资》(这样的课好像还有好多🤔). 虽然自己已经很久没有读, 更没有写资产定价方面的论文, 量化投资实践也~~几乎为0~~就是0, 但仍然对这个领域很感兴趣, 于是就接了这门课, 重新开始学习. 每周备课相当痛苦. 学生们要跟我这个半吊子一起学习一个学期, 虽然挺抱歉的, 也只能尽自己能力, 希望他们不会太过失望. 

### 因子简介
因子模型的代表作是Fama and French (1992)[^2], Fama and French (1993)[^3]. Fama 和 French 总结了前人的工作, 发现在美国股市1963到1991这段时间里, 如果把股票按照市值大小划分, 小市值的股票在未来的收益率比大市值股票更好；如果把股票按照市净率的倒数, Book-to-Market(简写为BM. Book是Book Value的简写, 也即资产负债表上计算出来的股东权益值；Market是Market Value的简写就是)划分, 高BM值的股票比低BM值的股票未来收益更好. 所谓size(市值)因子, 指的是用小市值Small的组合减去大市值Big的组合；而value(价值)因子，指的是价值股High组合减去成长股Low组合. 

这种通过找到某个变量排序，发现排序后的股票组合存在未来收益差距（时间上平均一下看是不是差距很显著）, 并以此构建投资组合的办法被广为应用. 这种构建的组合的收益率成为了所谓的“因子”——驱动个股收益率变化的“基本”因素. 所谓“基本”, 在金融学上也就是某种不可被分散的风险源. 金融理论认为这种风险源被投资者们识别, 并为此要求对应的风险报酬. 

动量(momentum)说的是"过去"一段时间里收益率较好的股票, "未来"收益率也较好. 反转(reversal)说的是"最近"收益率较好的股票, "将来"收益率较差. 有名的文章是Jegadeesh and Titman (1993)[^4], Jegadeesh (1990)[^5]. 

> 动量"过去": t月, 回看t-11到t-1个月
> 
> 动量"未来": t+1月, 或者更长时间比如半年甚至1年
> 
> 反转"最近": t月 
> 
> 反转"将来": t+1月 
> 
> 以上定义并没有定论，只是相对更常见

流动性(liquidity)因子说的是流动性差的股票, 未来有好的收益. 流动性这个概念很重要, 定义也五花八门. 这里我用的是最著名的Amihud (2002)[^6]“非流动性”指标, 也就是这个值越大, 流动性越差. 具体的定义是
$$Illiq_i = \frac{1}{D}\sum^D_{d=1} \frac{|R_{i,d}|}{VOLD_{i,d}},$$

其中\\(R_{i,d}\\)是股票\\(i\\)在\\(d\\)天的日收益率, \\(VOLD_{i,d}\\)是股票\\(i\\)在\\(d\\)天的成交额. 这个指标的直觉含义是, 如果很小的\\(VOLD_{i,d}\\)能形成很大的\\(R_{i,d}\\)的变化, 那么这个股票就是流动性比较差的. 在中国股市里, 这个指标比较大的就是所谓的“妖股”, 容易被坐庄, 小资金就能拉上拉下的.

异质性波动率(idiosyncratic volatility, ivol)指的是用因子模型对个股收益率做线性回归之后, 残差项的波动率. 也就是说, 个股收益率可以被分解为因子贡献的部分和其他部分. 残差项的波动率就是这个“其他部分”的波动率. 现有的发现是ivol大的, 未来的收益率更小, ivol小的, 未来的收益率更大. 这个现象和现有的金融理论相反. 相反的地方有两个. 一是异质性波动率按照理论来讲应当可以被分散掉, 不应该要求风险报酬. 二是即使可以认为现有的因子模型不够好, 异质性波动率仍包含了不能被分散掉的部分, 那么直觉上也应当是异质性波动率大的, 未来的收益更大, 因为这类股票风险更大, 要求更大的风险报酬. 参考文章是Ang, et al. (2006)[^7]

## 指标构建

1. size, value 的构建和 Fama French 基本一致. 不一样的地方是我的市值组合每月都进行更新, 而Fama French里面的的市值组合和价值组合一样, 都是一年更新一次. 
2. momentum 组合我进行了几种尝试, 都没有发现和未来收益正向相关. 相反, 负向相关性有一定的统计显著性. 尝试包括：t-11 到 t-1 的累积月收益率；t-3 到 t-1 的累积月收益率. 我给学生们布置的作业里让他们试了试 t-4 到 t-1 的累积周收益率, 结果显示也是负向相关的. 因此, 我们得出基本结论, 中国股市里动量效应不明显, 反转效应比较明显. reversal 这里选择的是过去1个月的收益率.
3. illiquidity 就是用的当月的 amihud 指标计算.
4. ivol用的是Fama-French 3 因子模型回归后的残差项的当月的波动率. 

以上所有的因子都是用“多空”组合构建的: 
1. 除了size本身, 其他因子都是把size先划分为2组之后, 再把指标划分为3组, 和size组合做交集. 这样总共有6组股票组合. 
2. 按照t月的指标对股票进行排序, 买入收益率高的组合, 卖出收益率低的组合, 持有到t+1月调仓. 

## 结果

图1应该已经能说明主要结果. 由于illiquidity股票涨得太疯, 把量纲拉得不成比例, 图2画了一个取log的结果. 

{{<figure src="/img/china_factors_log.png" title="图2">}}

如果从2017年开始画, 结果是这样的:
{{<figure src="/img/china_factors2017.png" title="图3">}}

可见因子择时也是很重要的。如果在2017年开始用size策略，表现会很差。但ivol似乎一直都还可以。

## 参考文献

[^1]:Liu, Jianan, Robert F. Stambaugh, and Yu Yuan, 2019, Size and value in China, Journal of Financial Economics 134, 48–69.

[^2]:Fama, Eugene F., and Kenneth R. French, 1992, The Cross-section of Expected Stock Returns, the Journal of Finance 47, 427–465.

[^3]:Fama, Eugene F., and Kenneth R. French, 1993, Common Risk Factors in the Returns on Stocks and Bonds, Journal of Financial Economics 33, 3–56.

[^4]:Jegadeesh, Narasimhan, and Sheridan Titman, 1993, Returns to Buying Winners and Selling Losers: Implications for Stock Market Efficiency, The Journal of Finance 48, 65–91.

[^5]:Jegadeesh, Narasimhan (1990), Evidence of Predictable Behavior of Security Returns. The Journal of Finance, 45, 881-898.

[^6]:Amihud, Yakov, 2002, Illiquidity and stock returns: cross-section and time-series effects, Journal of Financial Markets 5, 31–56.

[^7]:Ang, A., Hodrick, R.J., Xing, Y. and Zhang, X. (2006), The Cross‐Section of Volatility and Expected Returns. The Journal of Finance, 61: 259-299.