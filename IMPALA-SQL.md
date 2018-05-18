# Impala SQL snippets

---

### Extract a structured or nested column

Given a table with following schema

- id: Bigint
- name: String
- items: Array<Struct>
  - id: Bigint
  - title: String
  - price: Double

We can create a query which filters the array of struct as follows:

```
SELECT name, items.title as item_title, items.price as item_price
FROM table, table.items
WHERE items.price > 300
```


### Define the SELECTS beforehand

For complex query, it's more convenient to have multiple select expressions 
defined beforehand as follows.

```
WITH a AS (SELECT id, title, price FROM foo.bar 
          WHERE date BETWEEN '20180101' and '20180103'),
    b AS (SELECT id, title, status FROM foo.baz WHERE date >= '2018-01-01')
SELECT a.id, a.title, min(a.price) as min_price, max(a.price) as max_price,
      count(distinct b.status) as num_status
FROM a
JOIN b ON a.id == b.id
GROUP BY a.id
```


### Escape from nested case when

Avoid writing nested case when, better write:

```
SELECT id, name,
  COALESCE(
    CASE WHEN age < 5 THEN 'infant' ELSE NULL END,
    CASE WHEN age < 18 THEN 'teenager' ELSE NULL END,
    CASE WHEN age < 60 THEN 'adult' ELSE NULL END,
    'senior'
    ) AS age
FROM b
```


### Inspect table spec

With physical paths

```
SHOW TABLE STATS table
```


### Create structured column

Array of struct

```
CREATE TABLE table
(
  id BIGINT, name STRING,
  items ARRAY < STRUCT <
    category:STRING, 
    country_code:STRING, 
    area_code:SMALLINT,
    mobile:BOOLEAN
  > >
) STORED AS PARQUET;
```


### Refresh a newly re-created table

Once you drop a table and re-create it, Impala will fail 
to refresh it. Here is a sequence to bring it back into the services.

```
INVALIDATE METADATA tbl
REFRESH tbl
```



