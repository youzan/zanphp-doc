# Redis

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
yield Cache::set('aa.bb.cc', array|string $key, $value)
yield Cache::get('aa.bb.cc', array|string $key )
```

$key支持字符串和数据组。