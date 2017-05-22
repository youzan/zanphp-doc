Redis
=====

Using
~~~~~

the way to get or save data to Redis:

.. code:: php

    //可变参数
    yield Cache::$method($configKey, array|string $keys, ...);

-  $configKey: 见 `Redis <../../libs/pool/redis.md>`__\ 。
-  $key: $key be string or array. when $key is string , kv key equals
   $key. when $key is array,array replaces kvstore config's key
   placeholder,generate final key.see `KV <../../libs/pool/kv.md>`__.
-  The 2 previous parameters adapt all Cache Call(must be passed
   in),other parameters passed by different methods in
   order,\ ``...``\ meaning dynamic parameters。 parameters belong to
   Redis Official Documents，see `Redis Official
   Documents <http://redis.io/commands>`__\ 。

Redis Config
~~~~~~~~~~~~

config file in: resource/cache the first param in all methods of KV is
File Path.For example:
``yield KV::set('aa.bb.cc', ['foo', 'bar'], $value)`` meaning get cc
file of folder: recource/kvstore/aa/bb.

.. code:: php

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
