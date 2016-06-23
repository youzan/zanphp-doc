## Redis
获取redis连接可以采用连接池，服务启动时初始化好redis连接。

### 使用
> 通过框架中的Cache类存取数据，方式如下。
``` php
yield Cache::set('aa.bb.cc', $value, $key)
yield Cache::get('aa.bb.cc', $key)
```

$key支持字符串和数据组

