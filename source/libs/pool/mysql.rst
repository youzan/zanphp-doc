Mysql
=====

获取Mysql连接可以采用连接池，服务启动时初始化连接。

配置
~~~~

Mysql连接配置在项目 resource/config/$ENV/connection下。

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
            //连接池属性
            'pool'  => [
                //连接池中最大连接数目
                'maximum-connection-count' => 50,
                //连接池中最小连接数目
                'minimum-connection-count' => 1,
                //连接池心跳间隔时间
                'heartbeat-time' => 5000,
                //连接池初始连接数目
                'init-connection'=> 1,
            ],
        ],

    ];

注：连接池初始连接数目为init-connection和minimum-connection-count两者中的较大者。
