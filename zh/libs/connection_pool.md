## 连接池
> 包含了数据库、reids、kvstore、syslog等模块链接的管理。

###使用
连接池在服务启动时进行初始化，初始化成功后，通过 ConnectionManager类获取链接。
####代码
``` php
$connection = (yield ConnectionManager::getInstance()->get($database));
```
###配置
配置文件放在项目resource/config/connection 目录下，mysql为例，格式如下:
``` php
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
```


