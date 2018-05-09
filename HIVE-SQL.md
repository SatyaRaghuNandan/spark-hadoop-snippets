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



