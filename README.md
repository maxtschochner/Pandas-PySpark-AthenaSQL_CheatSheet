# Pandas-PySpark-AthenaSQL_CheatSheet

|-----------------|----------------------------------|----------------------------------|------------------------------------|
| Count Rows      | `df.shape[0]` or `len(df)`      | `df.count()`                     | `SELECT COUNT(*) FROM table;`    |
| Group By        | `df.groupby("col").sum()`       | `df.groupBy("col").sum()`        | `SELECT col, SUM(val) FROM table GROUP BY col;` |
| Aggregation     | `df.agg({"col": "sum"})`        | `df.agg({"col": "sum"})`         | `SELECT SUM(col) FROM table;`    |
| Multiple Aggs   | `df.groupby("col").agg({"val": ["sum", "mean"]})` | `df.groupBy("col").agg({"val": "sum", "val": "avg"})` | `SELECT col, SUM(val), AVG(val) FROM table GROUP BY col;` |

### Joining DataFrames
| Operation       | Pandas                                        | PySpark                                | AWS Athena SQL                         |
|----------------|----------------------------------------------|---------------------------------------|--------------------------------------|
| Inner Join     | `df1.merge(df2, on="col")`                  | `df1.join(df2, "col", "inner")`       | `SELECT * FROM df1 INNER JOIN df2 ON df1.col = df2.col;` |
| Left Join      | `df1.merge(df2, on="col", how="left")`       | `df1.join(df2, "col", "left")`        | `SELECT * FROM df1 LEFT JOIN df2 ON df1.col = df2.col;` |
| Right Join     | `df1.merge(df2, on="col", how="right")`      | `df1.join(df2, "col", "right")`       | `SELECT * FROM df1 RIGHT JOIN df2 ON df1.col = df2.col;` |
| Full Join      | `df1.merge(df2, on="col", how="outer")`      | `df1.join(df2, "col", "outer")`       | `SELECT * FROM df1 FULL OUTER JOIN df2 ON df1.col = df2.col;` |

### Handling Missing Data
| Operation       | Pandas                                        | PySpark                                | AWS Athena SQL                         |
|----------------|----------------------------------------------|---------------------------------------|--------------------------------------|
| Drop Nulls     | `df.dropna()`                               | `df.dropna()`                         | `SELECT * FROM table WHERE col IS NOT NULL;` |
| Fill Nulls     | `df.fillna(value)`                          | `df.fillna(value)`                    | `SELECT COALESCE(col, value) FROM table;` |

### Column Operations
| Operation       | Pandas                                        | PySpark                                | AWS Athena SQL                         |
|----------------|----------------------------------------------|---------------------------------------|--------------------------------------|
| New Column     | `df["new"] = df["col1"] + df["col2"]`       | `df.withColumn("new", df.col1 + df.col2)` | `SELECT col1 + col2 AS new FROM table;` |
| Conditional Col | `df["new"] = df["col"].apply(lambda x: x*2 if x > 0 else 0)` | `df.withColumn("new", when(df.col > 0, df.col*2).otherwise(0))` | `SELECT CASE WHEN col > 0 THEN col*2 ELSE 0 END AS new FROM table;` |

### Window Functions
| Operation       | Pandas (Rolling)                            | PySpark (Window)                     | AWS Athena SQL                         |
|----------------|----------------------------------------------|--------------------------------------|--------------------------------------|
| Moving Avg     | `df["rolling_avg"] = df["col"].rolling(3).mean()` | `df.withColumn("rolling_avg", avg("col").over(Window.rowsBetween(-2, 0)))` | `SELECT AVG(col) OVER (PARTITION BY id ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) FROM table;` |
