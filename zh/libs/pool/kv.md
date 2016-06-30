# KV

获取KV连接可以采用连接池，服务启动时初始化好KV连接。


###配置
KV连接配置在项目 resource/config/connection下。
``` php
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
```
