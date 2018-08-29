# Spark Snippets

---


### Nested whens

Instead of 

```scala
  df.withColumn("a", 
    when(condit1, lit(25)).otherwise(
    when(condit2, lit(0)).otherwise(
    when(condit3, lit(1)).otherwise(lit(-1)))))
```

do a coalesce trick for readability

```scala
  df.withColumn("a", coalesce(
    when(condit1, lit(25)),
    when(condit2, lit(0)),
    when(condit3, lit(1)),
    lit(-1)
  ))
```

The same technique applies to HiveSQL too


### Merge small partitions 

```scala
  sqlContext.parquetFile(INPUT).coalesce(SMALLER_NUM_PAR)
    .write.parquet(OUTPUT)
```

### Spark links

- (Spark's built-in basic stats exploration)[http://blog.madhukaraphatak.com/statistical-data-exploration-spark-part-1/]
