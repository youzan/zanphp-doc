## Redis
获取redis连接可以采用连接池，服务启动时初始化好redis连接。

### 使用
> 通过框架中的Cache类存取数据，方式如下。

``` php
yield Cache::set('aa.bb.cc', $value, $key)
yield Cache::get('aa.bb.cc', $key)
```

$key支持字符串和数据组。
###配置
redis连接配置在项目 resource/config/connection下。
``` php
<?php
return [
    'default_write' => [
        'engine'=> 'redis',
        'host' => '127.0.0.1',
        'port' => 6379,
        'pool'  => [
            'keeping-sleep-time' => '10',
            'init-connection'=> '2',
        ],
    ],
];
```

> Cache的配置文件位于 resource/cache下。Cache中 set和get方法的第一个参数表示文件路径，比如 Cache::set('aa.bb.cc', '',''),表示获取的是recource/cache/aa/bb文件下的cc。

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
//connection 表示redis连接配置，cache存数据的真实$key值是将cc中的属性key的$s替换成用户使用时传入的key的字符串，
//exp是过期时间，单位是s
```


