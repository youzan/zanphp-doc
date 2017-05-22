SqlMap
======

Target
------

target is make SQL transparence. DBA can look over SQL in biz,then make
SQL statement optimization, index optimization, and quickly locate the
slow queries. When new SqlMap version online, its can be pre-audit by
DBA,to avoid slow queries online.

Config
------

1. SqlMap Config

SqlMap file is in directory: resource/sql. the key value when called is
depend on directory structure.

2. Table Config

table file is in directory: resource/config/share/table/. the map file
is from tables of setting database.

.. code:: php

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

'mysql.default\_write' : book\_lottery、book\_lottery\_edit\_log ...
tables use config of 'default\_write' in:
resource/config/{$ENV}/connection/mysql.php.

why tables config alone? 1. Data Modeling 2. Sharding

Using
-----

SqlMap ：

.. code:: php

    <?php
    return [
        'row_by_market_id_goods_id'=>[
            'require' => ['market_id','goods_id'],
            'limit'   => [],
            'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id = #{goods_id} LIMIT 1',
        ]

php ：

.. code:: php

     <?php
       $data = [
           'var' => [
               'market_id' => $marketId,
               'goods_id' => $goodsIds
           ],
       ];
       $record = (yield Db::execute('market.marketGoods.row_by_market_id_goods_id', $data)); 

market.marketGoods.row\_by\_market\_id\_goods\_id ：

::

    market : resource/sql/market

    marketGoods : resource/sql/market/marketGoods.php

    row_by_market_id_goods_id : the SqlMap key value of marketGoods.php

Require
-------

All SQL write in SqlMap, tag #WHERE is not allowed. Example：

.. code:: php

       SELECT * FROM market_category WHERE market_id=#{market_id}  and parent_id= #{parent_id}  AND category_name= #{category_name}

Not allowed：

.. code:: php

       SELECT * FROM market_category WHERE #WHERE

SqlMap key Define Rules
-----------------------

SqlMap' key value word has '\_' prefix separator,that define the data
model of SQL return vales.

Example:the first word of 'row\_by\_market\_id\_goods\_id' is 'row'。

supports the following：

.. code:: php

    insert 单条插入 返回数据格式： int|0 值 last insert_id

    batch 多条插入 返回数据格式： bool

    update 更新 返回数据格式： bool

    delete 删除 返回数据格式： bool

    affected 获取影响行数 返回数据格式： int|0

    row 单行查询 返回数据格式： map|null 例： ['id' => 1, 'name' => 'xxxx']

    select 多行查询 返回数据格式： list|[] 例子 [['id' => 1, 'name' => 'xxx'], ['id' => 2, 'name' => 'xxx']]

    count 统计查询 返回数据格式： int|0

    raw 获取mysqli查询默认返回结果 返回数据格式： mixed

SqlMap Label Supported
----------------------

.. code:: php

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

uppercase except label field.

SQL grammar and Using
---------------------

insert
~~~~~~

.. code:: php

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

batch
~~~~~

.. code:: php

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

update
~~~~~~

.. code:: php

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

delete
~~~~~~

.. code:: php

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

affected
~~~~~~~~

.. code:: php

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

row
~~~

.. code:: php

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

select
~~~~~~

.. code:: php

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

count
~~~~~

.. code:: php

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

raw
~~~

.. code:: php

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

SqlMap Other Labels Using
-------------------------

order by
~~~~~~~~

.. code:: php

    //使用#ORDER#标签
    // SqlMap
    'raw_by_market_id_goods_ids' => [
        'require' => ['market_id','goods_id'],
        'limit'   => [],
        'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id IN #{goods_id} #ORDER# #LIMIT#',
    ]
    //调用
    $data = [
        'var' => [
            'market_id' => 222,
            'goods_id' => [333,111,333,555]，
        ],
        'order' => 'market_id DESC',
        'limit' => '0, 10'
    ];
    $record = (yield Db::execute('dir_name.file_name.raw_by_market_id_goods_ids', $data)); 

