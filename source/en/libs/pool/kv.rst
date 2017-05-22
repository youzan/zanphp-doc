KV
==

Get KV connection by Connection Pool. inited when service started.

Config
~~~~~~

KV connection config is in resource/config/$ENV/connection.

.. code:: php

    <?php
    return [
        'default' => [
            'engine'=> 'kVStore',
            //ip列表，将会对每一个建立链接
            'ip_list' => [
                '1,1,1,1:3000',
                '2.2.2.2:3000',
             ],
            'user' => '',
            'password' => '',
            'policy' => [
                'timeout' => 1000,
            ],
            'pool'  => [
                'keeping-sleep-time' => 10000,
                //初始化多少个链接，为0则不启用
                'init-connection'=> 0,
            ],
        ],
    ];
