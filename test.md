### 官方示例
- `<+>` \- L1 distance（曼哈顿距离，绝对差值之和）
- `<->` \- L2 distance（欧式距离，类似坐标系距离计算）
- `<#>` \- inner product（对应元素相乘后的和）
- `<=>` \- cosine distance（  A·B / |A|·|B|  ）

```sql
CREATE EXTENSION vector;

CREATE TABLE items (id bigserial PRIMARY KEY, embedding vector(3));

INSERT INTO items (embedding) VALUES ('[1,2,3]'), ('[4,5,6]')，('[7,8,9]');

-- L2 distance
SELECT * FROM items ORDER BY embedding <-> '[3,3,3]' LIMIT 5;

-- insert
INSERT INTO items (embedding) VALUES ('[6,6,6]');

-- update
UPDATE items SET embedding = '[3,2,1]' WHERE id = 1;

-- delete
DELETE FROM items WHERE id = 1;

-- Get the nearest neighbors to a vector
SELECT * FROM items ORDER BY embedding <-> '[2,2,2]' LIMIT 2;

-- 查看连接进程id
SELECT pg_backend_pid();

-- 禁用全表扫描
SET enable_seqscan = off;

-- 查看查询计划
EXPLAIN SELECT id, embedding, embedding <-> '[10, 20, 30]' AS distance
FROM items
ORDER BY embedding <-> '[10, 20, 30]'
LIMIT 10;

-- 构建索引
CREATE INDEX test_idx_1 ON a USING ivfflat (vec vector_l2_ops) WITH (lists = 100);

-- 查询索引
SELECT schemaname, tablename, indexname, indexdef
FROM pg_indexes
WHERE tablename = 'items';

-- 删除索引
drop index index_name;

CREATE INDEX test_idx ON a USING ivfflat (vec vector_l2_ops) WITH (lists = 100);

```
* * *
