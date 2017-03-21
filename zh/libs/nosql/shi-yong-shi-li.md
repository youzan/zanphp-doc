### 使用

> 通过框架中的Cache类存取数据，方式如下：

```php
//可变参数
yield Cache::$method($configKey, array|string $keys, ...);
```

* $method:redis的方法名称，如get、set等，redis的接口名称均可以直接使用。
* $configKey: 表示规则路径，比如 Cache::get\('aa.bb.cc', \['key1', 'key2'\]\),表示获取的是recource/cache/aa/bb文件下的cc。
* $key: $key可以为字符串或者数组，当$key为字符串时，redis最终执行的key为传入的$key, 当$key为array时，可按照array顺序替换cache config中的key占位符，生成最终的key。
* 前面2个参数对所有Cache调用固定\(必须传入\)，后面传入的参数需根据不同method所需参数的顺序传入，`...`意味这参数数量可变。method和后面可变参数顺序需按照redis官方文档顺序传入，见 [Redis官方文档](http://redis.io/commands)。

### 示例

```php
yield Cache::get("aa.bb.cc", ["zan", "test"])
```

resource/cache/aa/bb文件内容如上，则此语句等价于redis-&gt;get\("test\_\_abc\_zan\_\_test"\)。redis server的配置见[Redis](/zh/libs/pool/redis.md)。

