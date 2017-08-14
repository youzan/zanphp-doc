haunt.php
=========

文件位置
--------

resource/config/$ENV/haunt.php

配置作用
~~~~~~~~

服务发现、注册地址配置

配置内容（旧配置，建议使用新的\ `registry <../../html/config/registry.html>`__\配置）
---------------------------------------------------------------------------------------------------------

.. code:: php

    <?php
    return [
         // 拉取需要的服务列表，此处填写注册到注册中心的的app name，如果无需拉取任何服务，app_names为空array即可
        'app_names' => [
            'xxx-api',
        ],
        // 拉取app的详细配置，指定protocol和域
        'app_configs' => [
            // 从 com.xxx.service 域拉取scrm-api 服务
            'scrm-api' => [
                'protocol' => 'nova',
                'namespace' => 'com.xxx.service',
            ],
            // 从 com.xxx.test 域拉取pf-api 服务
            'pf-api' => [
                'protocol' => 'nova',
                'namespace' => 'com.xxx.test',
            ],
        ],
        //拉取服务配置 固定配置，业务无需修改
        'discovery' => [
            'host' => 'xxx.xxx.xxx.xxx',
            'port' => xxxx,
            'timeout' => 30000,
            'uri' => '/xx/xxx',
            'protocol' => 'nova',
            'namespace' => 'com.xxx.service', //固定配置与业务无关，下面配置同理
            'loop_time' => 1000,     //worker定时器任务执行时间（判断是否已拉取到服务）
        ],
        //监听服务配置 固定配置，业务无需修改
        'watch' => [
            'host' => 'xxx.xxx.xxx.xxx',
            'port' => xxxx,
            'timeout' => 30000,
            'uri' => '/xx/xxx',
            'protocol' => 'nova',
            'namespace' => 'com.xxx.service',
            'loop_time' => 5000,  //worker定时器任务执行时间（判断执行watch的worker是否live）
        ],
        //监听本地服务列表变化配置
        'watch_store' => [
            'loop_time' => 1000, //worker定时器任务执行时间(判断本地的服务列表是否变化)
        ],
        //服务注册配置 固定配置业务无需修改
        'register' => [
            'host' => '127.0.0.1',
            'port' => 9000,
            'uri' => 'uri',
            'timeout' => 30000,
            'protocol' => 'nova',
            'namespace' => 'com.xxx.service',
            'enable_register' => 1, //此处新加，0则不注册，1为注册，可以不填enable_register这个key，框架会默认注册
        ],
    ];

注意
~~~~

注册的服务发现列表需要确保已经成功注册至etcd
