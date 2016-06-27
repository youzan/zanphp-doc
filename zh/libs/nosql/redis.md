# Redis

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
yield Cache::$method($configKey, array|string $keys, ...);
```

$configKey说明见 ConfigKey说明

> Cache::method(