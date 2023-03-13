# Stocks
```dataview
TABLE 
	Sectors, 
	PE, 
	(round(medianPE * (estimatedEearningsGrowth + 1), 2)) AS EstimatedPE,
	(round((((round(medianPE * (estimatedEearningsGrowth + 1), 2)) - PE) / PE) * 100, 2)) AS PEDiff,
	MarketValue,
	medianPE AS MedianPE,
	estimatedEearningsGrowth AS EstimatedEearningsGrowth
FROM #stock   
Where MarketValue
SORT MarketValue DESC
```
# Sectors

```dataview
TABLE 
	PE, 
	(round(medianPE * (estimatedEearningsGrowth + 1), 2)) AS EstimatedPE,
	(round((((round(medianPE * (estimatedEearningsGrowth + 1), 2)) - PE) / PE) * 100, 2)) AS PEDiff,
	MarketValue,
	medianPE AS MedianPE,
	estimatedEearningsGrowth AS EstimatedEearningsGrowth
FROM #stockSector   
Where MarketValue
SORT MarketValue DESC
```

