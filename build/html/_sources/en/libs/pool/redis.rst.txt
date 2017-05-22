Redis
-----

Get reids connection by Connection Pool. inited when service started.

Config
~~~~~~

redis connection config in: resource/config/$ENV/connection下。

.. code:: php

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
