## 使用示例

### insert

```php
// SqlMap
'insert' => [
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

### batch insert

```php
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

```php
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

```php
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

```php
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

```php
// SqlMap
'row_by_market_id_goods_id' => [
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

```php
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

```php
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

```php
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

## SqlMap 其他标签使用方法

### order by

```php
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
```

### group by

```php
// SqlMap
'group_by_market_id' => [
    'require' => ['market_id','goods_id'],
    'limit'   => [],
    'sql'     => 'SELECT * FROM market_goods WHERE market_id = #{market_id} AND goods_id IN #{goods_id} #GROUP# #LIMIT#',
]
//调用
$data = [
    'var' => [
        'market_id' => 222,
        'goods_id' => [333,111,333,555]，
    ],
    'group' => 'market_id',
    'limit' => '0, 10'
];
$record = (yield Db::execute('dir_name.file_name.group_by_market_id', $data));
```



