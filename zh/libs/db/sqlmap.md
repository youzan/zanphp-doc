# SqlMap

## 目标

> 使用 SqlMap 主要目的是使我们的SQL语句可运维化，DBA可以直接查看我们的 SqlMap 了解我们业务中使用的SQL语句，对SQL语句进行优化，索引优化，还可以对慢查询进行快速定位。当业务发布新的 SqlMap 可以通过DBA预审核，避免出现线上慢查询。

## 配置

需要添加2个配置

1. SqlMap 配置

   SqlMap 文件需要放在 resource/sql 目录下，sql目录内部结构不限。目录结构决定了调用时的key值。

2. table配置

   table 需要在 resource/config/share/table/ 目录下建立项目数据库连接配置对应所有依赖的数据库表的map

```php
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

此处 mysql.default\_write 代表了 book\_lottery、book\_lottery\_edit\_log 等这些表的数据库配置是使用了 resource/config/{$ENV}/connection/mysql.php 里的 default\_write 的配置

为什么table要单独配置？

1. 数据建模
2. sharding

## 调用方式

SqlMap ：

```php
<?php
return [
    'row_by_market_id_goods_id'=>[
        'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1',
    ]
```

SqlMap主要定义sql生成的规则和约束，返回数组的键名标识一条sql生成规则。sql语句模板中的变量名以\#{var}的形式表示

php ：

```php
 <?php
   $data = [
       'var' => [
           'market_id' => $marketId,
           'goods_id' => $goodsIds
       ],
       'insert'=> [
            'market_id' => 1111,
            'goods_id' => 2222,
        ],
       'inserts' => [
            [
                'market_id' => 1111,
                'goods_id' => 2222,
            ],
            [
                'market_id' => 222,
                'goods_id' => 333,
            ],
        ],
       'limit' => "0, 2",
       'count' => 'market_id',
       'group' => 'goods_id',
       'order' => 'goods_id'       
   ];
   $record = (yield Db::execute('market.marketGoods.row_by_market_id_goods_id', $data));
```

market.marketGoods.row\_by\_market\_id\_goods\_id 解析：

* market 是目录名 resource/sql/market
* marketGoods 是文件名 resource/sql/market/marketGoods.php
* row\_by\_market\_id\_goods\_id 是 marketGoods.php 这个SqlMap里面的 key 值

$data数组解析：

* var用于替换sql模板中变量的值，数组键名与模板中的变量名一一对应
* insert在插入数据时使用，对应模板中的\#INSERT\#占位符
* inserts在批量插入数据时使用，对应模板中的\#INSERTS\#占位符
* limit替换sql模板中的\#LIMIT\#占位符为limit 0, 2
* count替换sql模板中的\#COUNT\#占位符为count\(market\_id\)
* group替换sql模板中的\#GROUP\#占位符为group by goods\_id
* order替换sql模板中的\#ORDER\#占位符为order by goods\_id

## 要求

所有SQL语句都要写在 SqlMap 里，非特殊情况，不允许使用\#WHERE 标签。必须要明确的SQL语句。  
 例子：

```php
   SELECT * FROM market_category WHERE market_id=#{market_id}  and parent_id= #{parent_id}  AND category_name= #{category_name}
```

禁止使用：

```php
   SELECT * FROM market_category WHERE #WHERE
```

## SqlMap key定义规则

SqlMap 的key值前缀分隔符 \_ 的首单词，定义了执行SQL以后返回的数据格式。

如 row\_by\_market\_id\_goods\_id 首个单词就是 row。

目前支持以下几种：

```php
insert 单条插入 返回数据格式： int|0 值 last insert_id

batch 多条插入 返回数据格式： bool

update 更新 返回数据格式： bool

delete 删除 返回数据格式： bool

affected 获取影响行数 返回数据格式： int|0

row 单行查询 返回数据格式： map|null 例： ['id' => 1, 'name' => 'xxxx']

select 多行查询 返回数据格式： list|[] 例子 [['id' => 1, 'name' => 'xxx'], ['id' => 2, 'name' => 'xxx']]

count 统计查询 返回数据格式： int|0

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

