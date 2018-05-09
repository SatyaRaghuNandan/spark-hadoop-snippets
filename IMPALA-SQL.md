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