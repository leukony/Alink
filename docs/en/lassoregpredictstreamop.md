## Description
Lasso regression predict stream operator. this operator predict data's regression value with linear model.

## Parameters
| Name | Description | Type | Required？ | Default Value |
| --- | --- | --- | --- | --- |
| reservedCols | Names of the columns to be retained in the output table | String[] |  | null |
| predictionCol | Column name of prediction. | String | ✓ |  |
| vectorCol | Name of a vector column | String |  | null |


## Script Example
#### Script
```python
data = np.array([
    [2, 1, 1],
    [3, 2, 1],
    [4, 3, 2],
    [2, 4, 1],
    [2, 2, 1],
    [4, 3, 2],
    [1, 2, 1],
    [5, 3, 3]])
df = pd.DataFrame({"f0": data[:, 0], 
                   "f1": data[:, 1],
                   "label": data[:, 2]})

batchData = dataframeToOperator(df, schemaStr='f0 int, f1 int, label int', op_type='batch')
colnames = ["f0","f1"]
lasso = LassoRegTrainBatchOp().setLambda(0.1).setFeatureCols(colnames).setLabelCol("label")
model = batchData.link(lasso)

predictor = LassoRegPredictStreamOp(model).setPredictionCol("pred")
predictor.linkFrom(batchData).print()
StreamOperator.execute()
```
#### Result
f0 | f1 | label | pred
---|----|-------|-----
 2 |  1     | 1 | 0.830304
   3 |  2    |  1 | 1.377312
   4 |  3    |  2 | 1.924320
   2 |  4    |  1 | 1.159119
   2 |  2    |  1 | 0.939909
   4 |  3    |  2 | 1.924320
   1 |  2    |  1 | 0.502506
   5 |  3    |  3 | 2.361724

