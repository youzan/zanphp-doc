# Log Connection Pool
Log connection pool used by [Log](../../lib/log.md)'s syslog type,long connection connected flume log system(Not Open Source Yet).<br>
Operation and Maintenance Engineer install flume client,and make local long connection.<br>
In Develop environment,install flume client in a public dev machine.

## Config Example
config files directory: resource/config/${ENV}/connection/syslog.php

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