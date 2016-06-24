# SqlMap


## 目标
> 使用 SqlMap 主要目的是使我们的sql语句可运维化，DBA可以直接查看我们的 SqlMap 了解我们业务中使用的sql语句，对sql语句进行优化，索引优化，还可以对慢查询进行快速定位。当业务发布新的 SqlMap 可以通过DBA预审核，避免出现线上慢查询。

## 配置
需要添加2个配置

1. SqlMap 配置 
  
  SqlMap 文件需要放在 resource/sql 目录下，sql内部结构不限。目录结构决定了调用是的key值。
  
2. table配置
 
  table 需要在 resource/config/share/table/ 目录下建立项目数据库链接配置对应所有依赖的数据库表的map

``` php
<?php
return [
    'mysql.default_write' => [
        'book_lottery',
        'book_lottery_edit_log',
        'book_lottery_prize',
        'book_lottery_user_join',
        'book_lottery_user_take',
        'buyer_notice',
        'entity_certification',
        'goods_collection_configure',
        'goods_collection_data',
        'goods_pf',
        'labels',
    ],    
];        
```
此处 mysql.default_write 代表了 book_lottery、book_lottery_edit_log 等这些表的数据库配置是使用了 resource/config/{$KDT_RUN_MODE}/connection/mysql.php 里的 default_write 的配置

## 调用方式
SqlMap ：
``` php
<?php
return [
    'row_by_market_id_goods_id'=>[
        'require' => ['market_id','goods_id'],
        'limit'   => [],
        'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1',
    ]
```
php ：
``` php
 <?php
   $data = [
       'var' => [
           'market_id' => $marketId,
           'goods_id' => $goodsIds
       ],
   ];
   $record = (yield Db::execute('market.marketGoods.row_by_market_id_goods_id', $data)); 
```
market.marketGoods.row_by_market_id_goods_id 解析：

market 是目录名 resource/sql/market

marketGoods 是文件名 resource/sql/market/marketGoods.php

row_by_market_id_goods_id 是 marketGoods.php 这个SqlMap里面的 key 值


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
```php
insert 单条插入 返回数据格式： int 值 last insert_id

batch 多条插入 返回数据格式： bool

update 更新 返回数据格式： bool

delete 删除 返回数据格式： bool

affected 获取影响行数 返回数据格式： int

row 单行查询 返回数据格式： map 例： ['id' => 1, 'name' => 'xxxx']

select 多行查询 返回数据格式： list 例子 [['id' => 1, 'name' => 'xxx'], ['id' => 2, 'name' => 'xxx']]

count 统计查询 返回数据格式： int

raw 获取mysqli查询默认返回结果 返回数据格式： mixed
```

## SqlMap 支持的标签

```php
  #INSERT#   
  #INSERTS#
  #DATA#
  #COLUMN#
  某字段名称 例如 kdt_id #{kdt_id} 或者#KDT_ID#
  #COUNT#
  #ORDER#
  #GROUP#
  #LIMIT#

  #WHERE 非必须不允许使用
  #AND 非必须不允许使用
  #OR 非必须不允许使用
```
除了字段的标签，其他都必须大写

## Sql语法

### insert
``` php
// SqlMap
'insert' => [
    'require' => [],
    'limit'   => [],
    'sql'     => 'INSERT INTO market_goods #INSERT#',
]

//调用
$data = [
    'insert' => [
        'market_id' => 1111,
        'goods_id' => 2222，
    ],
];
$record = (yield Db::execute('dir_name.file_name.insert', $data)); 

```
### batch

``` php
// SqlMap
'batch_insert'=>[
    'require' => [],
    'limit'   => [],
    'sql'     => 'INSERT INTO market_goods #INSERTS#',
]
//调用
$data = [
    'inserts' => [
        [
            'market_id' => 1111,
            'goods_id' => 2222，
        ],
        [
            'market_id' => 222,
            'goods_id' => 333，
        ],
    ]    
];
$record = (yield Db::execute('dir_name.file_name.inserts', $data)); 
```      

### update

``` php
// SqlMap
'update'=>[
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'UPDATE market_goods SET #DATA# WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1'
]    
//调用
$data = [
    'data' => [
        'name' => 1111,
        'time' => 2222，
    ],
    'var' => [
        'market_id' => 222,
        'goods_id' => 333，
    ],    
];
$record = (yield Db::execute('dir_name.file_name.update', $data)); 
```
### delete
``` php
// SqlMap
'delete' => [
    'require' => ['market_id','kdt_id','goods_id'],
    'limit'   => [],
    'sql'     => 'DELETE FROM market_goods WHERE market_id = #{market_id} AND kdt_id = #{kdt_id} AND goods_id = #{goods_id} LIMIT 1',
]
//调用
$data = [
    'var' => [
        'market_id' => 222,
        'goods_id' => 333，
    ],    
];
$record = (yield Db::execute('dir_name.file_name.delete', $data)); 

```

### affected

``` php
// SqlMap
'affected_update'=>[
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'UPDATE market_goods SET #DATA# WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1'
]    
//调用
$data = [
    'data' => [
        'name' => 1111,
        'time' => 2222，
    ],
    'var' => [
        'market_id' => 222,
        'goods_id' => 333，
    ],    
];
$record = (yield Db::execute('dir_name.file_name.affected_update', $data)); 
```

### row

``` php
// SqlMap
'row_by_market_id_goods_id' => [
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1',
]
//调用
$data = [
    'var' => [
        'market_id' => 222,
        'goods_id' => 333，
    ],
    'limit' => '0, 10'
];
$record = (yield Db::execute('dir_name.file_name.row_by_market_id_goods_id', $data)); 

```

### select
``` php
// SqlMap
'select_by_market_id_goods_ids' => [
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id IN #{goods_id} #LIMIT#',
]
//调用
$data = [
    'var' => [
        'market_id' => 222,
        'goods_id' => [333,111,333,555]，
    ],
    'limit' => '0, 10'
];
$record = (yield Db::execute('dir_name.file_name.select_by_market_id_goods_ids', $data)); 

```

### count
``` php
// SqlMap
'count_by_market_id_audit_status'=>[
    'require' => ['market_id','audit_status'],
    'limit'   => [],
    'sql'     => 'SELECT #COUNT# FROM market_goods WHERE market_id = #{market_id} AND audit_status = #{audit_status}',
]
//调用
$data = [
    'count' => '*',
    'var' => [
        'market_id' => 222,
        'audit_status' => 1，
    ],
];
$record = (yield Db::execute('dir_name.file_name.count_by_market_id_audit_status', $data)); 
```

### raw
``` php
// SqlMap
'raw_by_market_id_goods_ids' => [
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id IN #{goods_id} #LIMIT#',
]
//调用
$data = [
    'var' => [
        'market_id' => 222,
        'goods_id' => [333,111,333,555]，
    ],
    'limit' => '0, 10'
];
$record = (yield Db::execute('dir_name.file_name.raw_by_market_id_goods_ids', $data)); 

```
