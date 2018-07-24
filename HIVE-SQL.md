# Hive SQL snippets

---

### Clone a Hive table with selective data

Cloning a table to another with identical schema, 
but selecting only some data to copy across.

```
CREATE TABLE newtbl
AS SELECT * FROM oldtbl
WHERE <clause> 
```

### Drop partition

Simple

```
ALTER TABLE tabl DROP PARTITION (col=1)
```

### Tips : Pivot Sum without grouping

Example data:


| Name          | G             | Value |
| ------------- |:-------------:| -----:|
| abc           | A             |    50 |
| def           | B             |   125 |
| ghi           | B             |   225 |
| jkl           | B             |   300 |
| mno           | A             |     2 |

In case the data is huge and groupBy(G) is probably a costly operation, 
we can simply sum without a groupBy using a sign trick.

```
SELECT sum(case when G = "A" then v else 0 end value_a) as value_a
      ,sum(case when G = "B" then v else 0 end value_b) as value_b
FROM tbl
```

