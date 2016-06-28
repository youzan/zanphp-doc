# Redis

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
//可变参数
yield Cache::$method($configKey, array|string $keys, ...);
```

* $configKey: 见 [Redis](libs/pool/redis.md)。
* $key: $key可以为字符串或者数组，当$key为字符串时，redis最终执行的key为传入的$key, 当$key为array时，可按照array顺序替换cache config中的key占位符，生成最终的key。 见 [Redis](libs/pool/redis.md)。
* 前面2个参数对所有Cache调用固定(必须传入)，后面传入的参数需根据不同method所需参数的顺序传入，````...````意味这参数数量可变。method和后面可变参数顺序需按照redis官方文档顺序传入，见 [Redis官方文档](http://redis.io/commands)。

### Redis配置
> Cache的配置文件位于 resource/cache下。Cache中 set和get方法的第一个参数表示文件路径，比如 Cache::set('aa.bb.cc', ['key1', 'key2'], ''),表示获取的是recource/cache/aa/bb文件下的cc。

``` php
<?php
return [
    'common'           => [
        'connection'    => 'redis.default_write',
    ],
    'cc' => [
        'key' => 'test_abc_%s_%s',
        'exp' => 10
    ],
];
//connection 表示redis连接配置，cache存数据的真实$key值是将cc中的属性key的%s替换成用户使用时传入的key的字符串，
//exp是过期时间，单位是s
```