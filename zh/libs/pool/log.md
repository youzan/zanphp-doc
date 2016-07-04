# Log连接池

Log连接池仅供 [Log](../../lib/log.md) 的syslog类型使用，长连接连的是我们内部的flume日志系统（未开源）。运维同事在每台生产机器上部署了 flume 的 client端，所以长连接连接本地就可以了。开发环境可以考虑部署到一台公用的开发机器上。


## 配置示例
```PHP
<?php
 
return [
    'syslog_test' => [
        'engine'=> 'syslog',
        'host' => '127.0.0.1',
        'port' => '5140',
        'timeout' => 5000,
        'persistent' => true,
        'pool' => [
            'keeping-sleep-time' => 10000,
            'init-connection' => 2,
        ],
    ],
];
```