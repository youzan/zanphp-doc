Redis
=====

配置
~~~~

    Cache的配置文件位于
    resource/cache下，配置文件主要定义了连接池名称和key的生成规则。

resource/cache/aa/bb文件：

.. code:: php

    <?php
    return [
        'common'           => [
            'connection'    => 'redis.default_write',
            'local_cache' => [
                'enabled' => true,
                'ttl' => 10
            ],
        ],
        'cc' => [
            'key' => 'test_abc_%s_%s',
            'exp' => 10,
            'encode' => 'gz',
        ],
    ];

-  'common'字段为公共配置项，即数组中的cc等其他key共享common中的设置，上述配置等价于

.. code:: php

    <?php
    return [
        'cc' => [
            'key' => 'test_abc_%s_%s',
            'exp' => 10,
            'encode' => 'gz',
            'connection'    => 'redis.default_write',
        ],
    ];

-  connection
   表示redis连接配置，cache存数据的真实$key值是将cc中的属性key的%s替换成用户使用时传入的key的字符串
-  exp是过期时间，单位是s
-  encode表示redis数据压缩方式，目前只支持gz
-  local_cache是本地缓存配置，enable为true时代表开启本地缓存，ttl为本地缓存的超时时间,单位为s,默认ttl为3s。
    业务可根据自身需要开启本地缓存，减少频繁redis查询带来的开销。local_cache同样支持全局配置和key配置并存，
    key配置优先于全局配置，全局配置的方法见\ `server.php <../../config/server.html>`__\ 。


