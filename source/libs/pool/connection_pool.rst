连接池
------

    管理项目中需要使用连接池的模块，提升服务性能。

配置目录
~~~~~~~~

配置文件放在项目resource/config/connection
目录下，目前已支持的engine类型有mysqli、redis、syslog、novaClient、kVStore。

使用
~~~~

连接池在服务启动时进行初始化，初始化成功后，通过
ConnectionManager类获取连接。

获取连接
^^^^^^^^

.. code:: php

    $connection = (yield ConnectionManager::getInstance()->get($database));

`database名称由文件名+属性名组成，如resource/config/connection/mysql.php包括两种连接池属性。`\ database分别是mysql.default\_write和mysql.default\_read。

.. code:: php

    <?php
    return [
        'default_write' => [
            'engine'=> 'mysqli',
            'host' => '127.0.0.1',
            'user' => '',
            'password' => '',
            'database' => '',
            'port' => '3306',
            'pool'  => [
                'maximum-connection-count' => 50,
                'minimum-connection-count' => 1,
                'heartbeat-time' => 5000,
                'init-connection'=> 1,
            ],
        ],
        'default_read' => [
            'engine'=> 'mysqli',
            'host' => '127.0.0.1',
            'user' => '',
            'password' => '',
            'database' => '',
            'port' => '3306',
            'pool'  => [
                'maximum-connection-count' => 50,
                'minimum-connection-count' => 1,
                'heartbeat-time' => 5000,
                'init-connection'=> 1,
            ],
        ],

    ];
