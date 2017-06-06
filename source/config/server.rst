server.php
==========

文件位置
~~~~~~~~

::

    resource/config/$ENV/server.php

配置作用
~~~~~~~~

Server相关配置

配置文件内容
~~~~~~~~~~~~

.. code:: php

    <?php

    return [
        //监听host, 默认0.0.0.0即可
        'host'          => '0.0.0.0',
        //监听ip
        'port'          => '8020',
        'config' => [
            //worker数量，推荐设置和cpu核数相等
            'worker_num' => 2,
            //reactor数量，推荐设置和cpu核数相等
            'reactor_num' => 2,
            //以下配置直接复制，无需改动
            'open_length_check' => 1,
            'package_length_type' => 'N',
            'package_length_offset' => 0,
            'package_body_offset' => 0,
            'open_nova_protocol' => 1,
            'package_max_length' => 2000000,
        ],
        //worker进程监控重启参数
        'monitor' =>[
            //进程请求多少次后重启
            'max_request'   => 100000,          //
            //进程存活多久之后重启
            'max_live_time' => 1800000,         //30m
            //多久检查一次
            'check_interval'=> 10000,           //10s
            //内存占用多大后重启
            'memory_limit'  => 1.5 * 1024 * 1024 * 1024,       //1.50G
            //cpu占用多少之后重启
            'cpu_limit'     => 70,
            //是否开启monitor debug模式
            'debug'         => false,
            //单个worker进程最大并发数(流控)
            'max_concurrency' => 2000,
        ],
        //请求超时时间, 单位ms
        'request_timeout' => 30000,
        //监控上报参数
        'hawk_collection' => [
            'enable_hawk' => 0, //是否开启监控采集 1开启，0不开启
            'hawk_url' => 'http://www.example.com',
        ],
        //信任的server反向代理ip，只有从受信任的代理ip转发至server的包纳入真实客户端ip地址统计
        'proxy' => [
            'a.a.a.a',
            'b.b.b.b',
        ],
    ];
