# SqlMap


## 目标
> 使用 SqlMap 主要目的是使我们的sql语句可运维，运维部门可以直接查看我们的 SqlMap 了解我们业务中使用的sql语句，对sql语句进行优化，索引优化，还可以对慢查询进行快速定位。当业务发布新的 SqlMap 可以通过运维预审核，避免出现线上慢查询。

## 配置
SqlMap 文件需要放在 resource/sql 目录下，sql内部结构不限。目录结构决定了调用是的key值。

## 调用方式

``` php
 <?php
   $record = (yield Db::execute('market.marketGoods.row_by_market_id_goods_id', $data)); 
```
market.marketGoods.row_by_market_id_goods_id 解析：

market 是目录名 resource/sql/market

marketGoods 是文件名 resource/sql/market/marketGoods.php

row_by_market_id_goods_id 是 marketGoods.php 这个SqlMap里面的 key 值

``` php
<?php
return [
    'row_by_market_id_goods_id'=>[
        'require' => ['market_id','goods_id'],
        'limit'   => [],
        'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1',
    ]
```

## 要求
 所有sql语句都要写在 SqlMap 里，非特殊情况，不允许使用#WHERE 标签。必须要明确的sql语句。
 例子：
``` php
   SELECT * FROM market_category WHERE market_id=#{market_id}  and parent_id= #{parent_id}  AND category_name= #{category_name}
```
禁止使用：
``` php
   SELECT * FROM market_category WHERE #WHERE
```

## SqlMap key定义规则

SqlMap 的key值前缀分隔符 _ 的首单词，定义了执行sql以后返回的数据格式。

如 row_by_market_id_goods_id 首个单词就是 row。

目前支持以下几种：

insert 单条插入 返回数据格式： int 值 last insert_id

batch 多条插入 返回数据格式： bool

update 更新 返回数据格式： bool

delete 删除 返回数据格式： bool

affected 获取影响行数 返回数据格式： int

row 单行查询 返回数据格式： map 例： ['id' => 1, 'name' => 'xxxx']

select 多行查询 返回数据格式： list 例子 [['id' => 1, 'name' => 'xxx'], ['id' => 2, 'name' => 'xxx']]

count 统计查询 返回数据格式： int

raw 获取mysqli查询默认返回结果 返回数据格式： mixed
## Sql语法

### insert
``` php
'insert' => [
    'require' => [],
    'limit'   => [],
    'sql'     => 'INSERT INTO market_goods #INSERT#',
],
```
### batch
``` php
'batch_insert'=>[
    'require' => [],
    'limit'   => [],
    'sql'     => 'INSERT INTO market_goods #INSERTS#',
],
```      
### update
``` php
'update'=>[
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'UPDATE market_goods SET #DATA# WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1'
],
```
### delete
``` php

```
### affected
``` php

```
### row
``` php

```

### select
``` php

```
### count
``` php

```

### raw
``` php

```


