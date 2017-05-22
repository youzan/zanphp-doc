Connection Pool
---------------

    management requires the use of connection pooling module, improve
    service performance.

Using
~~~~~

init in service starting,connect by ConnectionManager.

Get Connection
^^^^^^^^^^^^^^

.. code:: php

    $connection = (yield ConnectionManager::getInstance()->get($database));

Config
~~~~~~

config files are in resource/config/connection. For example, MySQL
config:

.. code:: php

    <?php
    return [
        'default_write' => [
            'engine'=> 'mysqli',
            'host' => '127.0.0.1',
            'user' => 'user',
            'password' => 'password',
            'database' => 'db',
            'port' => '3306',
            'pool'  => [
                'heartbeat-time' => 35000,
                'init-connection'=> 5,
            ],
        ]
    ];

Supported Engine: mysqli、redis、syslog、novaClient、kVStore.
