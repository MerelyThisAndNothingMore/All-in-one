# 大盘
大盘连跌一周

# 关注股票
```dataview
TABLE Evaluation, Turnover, PE, MarketValue, Sectors
FROM #stockInFocus  
WHERE Evaluation
SORT MarketValue DESC
```

