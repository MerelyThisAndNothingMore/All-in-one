---
tags: 
alias:
---
1. 板块优先
2. 低买
# 优势板块






















# StockTransaction
```dataview
TABLE 
	tradingStock,
	profit,
	profitability,
	sellDate - buyDate as HoldingTime
FROM #stockTransaction 
WHERE profit 
SORT buyDate ASC
```
