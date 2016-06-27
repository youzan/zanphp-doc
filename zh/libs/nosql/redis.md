# Redis

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
//可变参数
yield Cache::$method($configKey, array|string $keys, ...);
```

* $configKey: 见 [Redis](libs/pool/redis.md)
* $key: $key可以为字符串或者数组，当$key为字符串时，redis最终执行的key为传入的$key, 当$key为array时，可按照array顺序替换cache config中的key占位符，生成最终的key。 见 [Redis](libs/pool/redis.md)
* 前面2个参数对所有Cache调用固定，后面传入的参数需根据不同method所需参数的顺序传入，````...````意味这参数数量可变。method和后面可变参数顺序需按照redis官方文档顺序传入，见 [Redis官方文档](http://redis.io/commands)。

