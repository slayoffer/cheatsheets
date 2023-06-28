### DB speed test string

```bash
select COUNT(*) from orders o INNER JOIN order_product op ON o.id = op.order_id INNER JOIN product p ON op.product_id = p.id WHERE p.id = 4;
```

### Create index

```sql
CREATE INDEX index_name ON table_name [USING method]
(
    column_name [ASC | DESC] [NULLS {FIRST | LAST }],
    ...
);

# method — алгоритм создания указателей (по умолчанию btree)
# [ASC | DESC] [NULLS {FIRST | LAST }] — порядок сортировки (по умолчанию ASC, NULLS FIRST)

# example
CREATE INDEX idx_name_of_index ON table_name USING btree (table_parameter1, table_parameter2) 

# check current indexes
SELECT tablename, indexname, indexdef
FROM pg_indexes
WHERE schemaname = 'public'
ORDER BY tablename, indexname;  
```

### Indexes example

schemaname |   tablename   | indexname | indexdef |
-----------|---------------|-----------|--------------------------------------------------------------------------------|
public     | order_product | o_p_idx   | CREATE INDEX o_p_idx ON public.order_product USING btree (product_id, order_id)| 
public     | orders        | oid_idx   | CREATE INDEX oid_idx ON public.orders USING btree (id)                         |

