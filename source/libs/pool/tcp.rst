Tcp
---

Tcp连接池主要用于调用链，服务启动时初始化连接。

配置
~~~~

tcp连接配置在项目 resource/config/$ENV/connection下。

.. code:: php

    <?php
    return [
        'trace' => [
            'engine'=> 'tcp',
            'host' => '127.0.0.1',
            'port' => '2280',
            'timeout' => 5000,
            'hasRecv' => false,
            'config'    => [
                'open_length_check' => 1,
                'package_length_type' => 'N',
                'package_length_offset' => 0,
                'package_body_offset' => 0,
                'open_nova_protocol' => 1
            ],
            'pool'  => [
                'maximum-connection-count' => 50,
                'minimum-connection-count' => 50,
                'keeping-sleep-time' => '10',
                'init-connection'=> 50,
            ],
        ],
    ];
