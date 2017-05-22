KV
==

Using
~~~~~

get KV data by KV Class, using:

.. code:: php

     $res = (yield KV::set(string $configKey, array|string $key, $value));

-  $configKey: see `KV <../../libs/pool/kv.md>`__\ 。
-  $key: $key be string or array. when $key is string , kv key equals
   $key. when $key is array,array replaces kvstore config's key
   placeholder,generate final key. see `KV <../../libs/pool/kv.md>`__.

KV Config
~~~~~~~~~

config file in: resource/kvstore the first param in all methods of KV is
File Path.For example:
``yield KV::set('aa.bb.cc', ['foo', 'bar'], $value)`` meaning get cc
file of folder: recource/kvstore/aa/bb.

.. code:: php

    <?php
    return [
        'common'           => [
            'connection'    => 'kv.default_write',
        ],
        'cc' => [
            'key' => 'test_abc_%s_%s',
            'exp' => 10
        ],
    ];
    //connection 表示kv连接配置，kv存数据的真实$key值是将cc中的属性key的%s替换成用户使用时传入的key的字符串，
    //exp是过期时间，单位是s
