# Mysql
Get Mysql connection by Connection Pool. inited when service started.


### Config
Mysql connection Config in: resource/config/$ENV/connection.
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

