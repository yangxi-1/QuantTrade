# QuantTrade
This is my exploration an QuantTrading

寻找正确性大于0.5的赚钱策略，并严格遵守交易策略。

#股市markdov模型

牛市－－>猴市－－>熊市(－－>猴市)－－>牛市

   
思路一：结合经验性指标操作

思路二：强化学习，让agent自己学习买卖时机（指标为state，收益为reward）

# 思路一：经验性指标操作 
## 策略：低买高卖
关键在于如何判断低点还是高点

第一步，大行情的判断，当大行情到来时能够抓住。

第二步，判断领涨板块，并在领涨板块中选择龙头股。

第三步，对于领涨股进行操作，买卖（结合低点高点判断）。

第四步，主升浪结束，领涨股到顶，对于低价潜力股进行寻找。

第五步，市场进入震荡期，小区间低买高卖，并结合指标进行风险性评估，风险过高时果断清仓（如成交量日益萎缩、主力资金持续流出，意味着这波行情的收尾）。

第六步，下跌初期，按兵不动。

第七步，下跌一段时间后（如前期行情的50%），判断阶段性底部和全局底部。（主要依据上涨趋势的强弱、以及突破关键阻力位的速度）

### 大行情启动的判断：
- 估值
	- 所有行业指数普跌到低估区间
	- 平均市盈率处于低估区间
- 成交量
	- 周线成交量极度萎缩
	- 相比于上一周，萎缩30%（大概率会有，但不绝对）
	- 大盘成交金额在万亿以下
- 技术指标
	- kdj周期性金叉
	- 周线形态十字星居多（周期性横盘震荡）
- 市场情绪
	- 低迷、冰点



### 判断领涨板块和龙头股（行情初期）
选择涨幅最高板块内的涨速涨幅最高的股票。


### 判断低价股中的潜力股（行情中后期）
目标：去找有热度且低价的票子
- 成交额
	- 明显高于大环境下其他票子成交额的
	- 中期50亿，后期10－20亿
- 涨幅
	- 相比于大节点启动日的累计涨幅排名相对较低的
- k线
	- 四连阳、五连阳
	- 大阳线越多越好
	- 调整期间上下影线越短越好-->反应控盘资金实力
	- 振幅越小越好-->反应控盘资金实力
- 人气
	- 同花顺、东方财富人气靠前的
- 市值
	- 启动时市值较小的，10块以内启动的

### 对所选股票进行操作（在大行情中的操作）
分别寻找低点、高点的指标判断准则，并利用准则对当前情况进行判断。因为我们的目的是最大化收益，要充分结合风险好回报，力求卖在尽量高的点，买在尽量低的点。

#分别实现以下四个指标的判断： 局部最高点、局部最低点、全局最高点、全局最低点。<mark>对这些点判断的准确率直接关乎着策略回撤的大小。</mark>



首先是局部最值得判断，局部最值中将产生全局最值，全局最值是用局部最值进行更新的。

<mark>声明：接下来的所有分析都是基于能够检测出所有的局部最优点而产生的（可能会误判、但不会漏检）</mark>

因此，在局部最值处有两种情况：第一，此处只是回调点；第二，此处为最值点；第三，此处不是局部最值，只是上涨或下跌的一个中间点。

对于局部高点，上面的第一第二种情况可以采取一样的最优策略：清仓。第三种情况，显然继续全仓是最优策略。

**方案一**： 兼顾风险和收益，在这个点部分清仓。
- 这个点为高点（第一、第二种情况），在下跌一定程度后形成一个关键点时进行判断：分歧点
- 这个点被误判，则在接下来的一个分歧点清仓。


**方案二**：保守型，在这个点清仓。
- 这个点为高点，清仓
- 这个点被误判，在接下来被证实为误判后重新满仓。

对于局部低点，类似可知。

**总结一下两个方案：** 方案一的操作比较折中，适合于对于局部最优点判断精度一般时。方案二的操作比较激进，适用于对于局部最优点判断很精确时。





