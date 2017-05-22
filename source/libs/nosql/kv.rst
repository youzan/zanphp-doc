KV
==

使用
~~~~

    通过框架中的KV类存取数据，方式如下。

.. code:: php

     $res = (yield KV::set(string $configKey, array|string $key, $value));

-  $configKey: 见 `KV <../../libs/pool/kv.md>`__\ 。
-  $key:
   :math:`key可以为字符串或者数组，当`\ key为字符串时，kv最终执行的key为传入的\ :math:`key, 当`\ key为array时，可按照array顺序替换kvstore
   config中的key占位符，生成最终的key。 见
   `KV <../../libs/pool/kv.md>`__\ 。

KV配置
~~~~~~

    KV的配置文件位于
    resource/kvstore下。KV中所有方法的第一个参数表示文件路径，比如
    ``yield KV::set('aa.bb.cc', ['foo', 'bar'], $value)``,表示获取的是recource/kvstore/aa/bb文件下的cc。

.. code:: php

    <?php
    return [
        'common'           => [
            'connection'    => 'kv.default_write',
        ],
        'cc' => [
            'key' => 'test_abc_%s_%s',
            'exp' => 10,
            'namespace' => 'youzan',
            'set' => 'aaaatest'
        ],
    ];

-  'common'字段为公共配置项，即数组中的cc等其他key共享common中的设置，上述配置等价于

.. code:: php

    <?php
    return [
        'cc' => [
            'key' => 'test_abc_%s_%s',
            'exp' => 10,
            'namespace' => 'youzan',
            'set' => 'aaaatest',
            'connection'    => 'kv.default_write',
        ],
    ];

-  connection
   表示kv连接配置，kv存数据的真实$key值是将cc中的属性key的%s替换成用户使用时传入的key的字符串，
-  exp是过期时间，单位是s
-  namespace是kvstore中的命名空间
-  set是kvstore中的set字段
