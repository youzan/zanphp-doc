# Redis

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
//可变参数
yield Cache::$method($configKey, array|string $keys, ...);
```

$configKey: [ConfigKey说明](libs/pool/redis.md)
$key: $key可以为字符串或者数组，当$key为字符串时，redis命令的key

> Cache::method(