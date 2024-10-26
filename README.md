# Predict-US-stocks-closing-movements

![image](https://github.com/Tony980624/Predict-US-stocks-closing-movements/blob/main/file01/header.jpg)

竞赛目标 :开发一个模型，能够使用来自股票订单簿和收盘拍卖的数据预测数百家纳斯达克上市公司股票的收盘价格走势。拍卖信息可以用来调整价格，评估供需动态，并识别交易机会。

# 取得结果

![result](https://github.com/Tony980624/Predict-US-stocks-closing-movements/blob/main/file01/Tony980%20-%20Optiver%20-%20Trading%20at%20the%20Close.png)

# 训练数据变量解读

- **stock_id** - 股票的唯一标识符。并非所有股票ID在每个时间桶中都存在。
- **date_id** - 日期的唯一标识符。日期ID在所有股票中是连续且一致的。
- **imbalance_size** - 当前参考价格下未匹配的金额（以美元计）。
- **imbalance_buy_sell_flag** - 反映拍卖不平衡方向的指标。
  - `1` 表示买方不平衡。
  - `-1` 表示卖方不平衡。
  - `0` 表示无不平衡。
- **reference_price** - 在此价格下，匹配的股份最大化，不平衡最小化，且与买卖叫价中点的距离最小化。这个价格也可以看作是最佳买价和最佳卖价之间的近似价格。
- **matched_size** - 当前参考价格下可以匹配的金额（以美元计）。
- **far_price** - 基于仅拍卖兴趣的最大化股份匹配的交叉价格。此计算不包括连续市场订单。
- **near_price** - 基于拍卖和连续市场订单的最大化股份匹配的交叉价格。
- **[bid/ask]_price** - 非拍卖账本中最具竞争力的买/卖级别的价格。
- **[bid/ask]_size** - 非拍卖账本中最具竞争力的买/卖级别的美元名义金额。
- **wap** - 非拍卖账本中的加权平均价格。
- **seconds_in_bucket** - 自当天收市拍卖开始以来经过的秒数，始终从0开始。
- **target** - 股票WAP的未来60秒移动减去合成指数的未来60秒移动。仅在训练集中提供。
- 合成指数是 Optiver 为此竞赛构建的基于纳斯达克上市股票的自定义加权指数。
- 目标值的单位是基点，这是金融市场中常用的度量单位。1基点的价格移动等同于0.01%的价格移动。
- 在当前观测时刻t，目标可以定义为：

  
![target](https://github.com/Tony980624/Predict-US-stocks-closing-movements/blob/main/file01/target.png)

All size related columns are in USD terms.

# 估算出每只股票对值数的影响权重

## 线性回归估算合成指数权重

### 数据加载和预处理
- 使用 `pandas` 读取股票数据。
- 计算每只股票在不同日期和时间桶内的回报率 (`stock_return`)。
- 从股票回报率中扣除目标值 (`target`) 得到指数回报率 (`index_return`)。

### 构建矩阵
- `stock_returns` 和 `index_returns` 初始化为零矩阵，存储每只股票在每个日期和时间桶的回报率。
- 遍历每只股票和日期的组合，计算回报率和调整后的指数回报率。

### 线性回归模型
- 使用 `LinearRegression` 来拟合数据，使模型预测的指数回报率与实际计算的指数回报率尽可能接近。
- 自变量 X 是股票的回报率，因变量 y 是指数的回报率。
- 回归模型找到的系数（权重）反映每只股票对指数影响的相对重要性。

### 模型评估
- 输出回归系数（每只股票的权重）、截距和 R² 值（决定系数，衡量模型拟合质量的指标）。
- R² 值接近 1 表示模型拟合得非常好，几乎可以完美预测指数回报率。

### 权重的实际应用
- 这些权重帮助投资者或基金经理了解哪些股票对指数波动的贡献最大，从而进行更有针对性的投资决策。


