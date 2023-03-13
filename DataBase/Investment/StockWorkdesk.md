---
SCIPE: 13.09
SCIMedianPE: 13.52
---
CalibrationFactor:: `= round(this.SCIPE / this.SCIMedianPE, 3)`
# Stocks
```dataview
TABLE 
	Sectors, 
	PE, 
	(round(medianPE * (estimatedEearningsGrowth + 1), 2)) AS EstimatedPE,
	MarketValue,
	medianPE AS MedianPE,
	estimatedEearningsGrowth AS EstimatedEearningsGrowth
FROM #stockInFocus  
Where MarketValue
SORT MarketValue DESC
```
# Sectors

