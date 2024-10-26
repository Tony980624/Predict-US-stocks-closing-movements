# Predict-US-stocks-closing-movements

*竞赛目标 :开发一个模型，能够使用来自股票订单簿和收盘拍卖的数据预测数百家纳斯达克上市公司股票的收盘价格走势。拍卖信息可以用来调整价格，评估供需动态，并识别交易机会。

*取得结果

![result](https://github.com/Tony980624/Predict-US-stocks-closing-movements/blob/main/file01/Screenshot%202024-10-26%20111914.png?raw=true)

*训练数据解读

- **stock_id** - A unique identifier for the stock. Not all stock IDs exist in every time bucket.
- **date_id** - A unique identifier for the date. Date IDs are sequential & consistent across all stocks.
- **imbalance_size** - The amount unmatched at the current reference price (in USD).
- **imbalance_buy_sell_flag** - An indicator reflecting the direction of auction imbalance.
  - `1` for buy-side imbalance.
  - `-1` for sell-side imbalance.
  - `0` for no imbalance.
- **reference_price** - The price at which paired shares are maximized, the imbalance is minimized, and the distance from the bid-ask midpoint is minimized. It can also be considered as equal to the near price bounded between the best bid and ask price.
- **matched_size** - The amount that can be matched at the current reference price (in USD).
- **far_price** - The crossing price that will maximize the number of shares matched based on auction interest only. This calculation excludes continuous market orders.
- **near_price** - The crossing price that will maximize the number of shares matched based on auction and continuous market orders.
- **[bid/ask]_price** - Price of the most competitive buy/sell level in the non-auction book.
- **[bid/ask]_size** - The dollar notional amount on the most competitive buy/sell level in the non-auction book.
- **wap** - The weighted average price in the non-auction book. Calculated as:
- **seconds_in_bucket** - The number of seconds elapsed since the beginning of the day's closing auction, always starting from 0.
- **target** - The 60 second future move in the WAP of the stock, less the 60 second future move of the synthetic index. Only provided for the train set.
- The synthetic index is a custom weighted index of Nasdaq-listed stocks constructed by Optiver for this competition.
- The unit of the target is basis points, which is a common unit of measurement in financial markets. A 1 basis point price move is equivalent to a 0.01% price move.
- Where t is the time at the current observation, the target can be defined as:
  
![target]()



All size related columns are in USD terms.

