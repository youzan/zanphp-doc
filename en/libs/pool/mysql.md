# Mysql
获取Mysql连接可以采用连接池，服务启动时初始化连接。


###配置
Mysql连接配置在项目 resource/config/$ENV/connection下。
``` php
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

];
```

