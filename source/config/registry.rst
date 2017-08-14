registry.php
=============

文件位置
--------

resource/config/$ENV/registry.php

配置作用
~~~~~~~~

服务注册中心配置：服务注册，服务发现，服务订阅

配置内容(新配置，用于替代\ `nova <../../html/config/nova.html>`__\和\ `haunt <../../html/config/nova.html>`__\配置文件)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code:: php

    <?php

    return [
        "enable" => true,

        // registry 类型
        "type" => "etcd", // haunt

        // -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
        // 服务发现与订阅

        // 服务发现配置
        "discovery" => [
            "timeout"=> 3000,
            "loop_time" => 1000,     //worker定时器任务执行时间（判断是否已拉取到服务）
        ],

        //监听服务变更配置 worker#0
        "watch" => [
            "timeout" => 30000, // etcd watch 长轮询超时时间
            "loop_time" => 5000,  //worker定时器任务执行时间（判断执行watch的worker是否live）
        ],

        //监听apcu服务列表变更配置 worker#1...n
        "watch_store" => [
            "loop_time" => 1000, //worker定时器任务执行时间(判断本地的服务列表是否变化)
        ],

        // -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
        // 服务拉取

        // 配置从注册中心拉取的服务名称
        "app_names" => [
        //    "tcp-demo"
        ],

        // 配置从注册中心拉取服务的协议(暂时只能是nova)与命名空间(域)
        "app_configs" => [
            "tcp-demo" => [
                "protocol" => "nova",
                'namespace' => 'com.test.service',
            ],
        ],

        // -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
        // 服务发布

        // 发布单组 nova 服务
        "novaApi" => [
            //nova-service的vendor目录，直到gen-php
            "path"  => "vendor/nova-service/nova-demo/sdk/gen-php",
            //gen-php文件夹对应的namespace
            "namespace" => "Com\\Youzan\\Demo\\",

            // 可选, 默认 com.youzan.service, 配置服务发布到 具体的域, 与app_configs.${$app_name}.namespace 对应
            "domain" => 'com.youzan.service',
            // 可选, 默认Application::getName(), 配置服务发布的 应用名
            "appName"   => "tcp-demo",
            // 可选, 目前恒等于 nova
            "protocol"   => "nova",
        ],

        // 发布多组 nova 服务
        /*
        "novaApi" => [
            [
                "path"  => "vendor/nova-service/xxx/gen-php",
                "namespace" => "Com\\Youzan\\Xxx\\",
                "domain" => "com.youzan.service", // 可选, 默认 com.youzan.service, 配置服务发布到 具体的域
                "appName"   => "Xxx", // 可选, 默认Application::getName(), 配置服务发布的 应用名
                "protocol"   => "nova", // 可选, 目前恒等于 nova
            ],
            [
                "path"  => "vendor/nova-service/yyy/gen-php",
                "namespace" => "Com\\Youzan\\Yyy\\",
                "domain" => "com.youzan.service", // 可选, 默认 com.youzan.service, 配置服务发布到 具体的域
                "appName"   => "Yyy", // 可选, 默认Application::getName(), 配置服务发布的 应用名
                "protocol"   => "nova", // 可选, 目前恒等于 nova
            ],
        ],
        */

        // -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
        // etcd

        // 配置etcd集群节点, 可配置多个
        "etcd" => [
            "nodes" => [
                [
                    "host" => "127.0.0.1",
                    "port" => 2379,
                ],
            ],
        ],

        "haunt" => [

        ],
    ];

注意
~~~~

原本 nova.php haunt.php 配置全部合并到 registry.php, 兼容旧配置

注册的服务发现列表需要确保已经成功注册至etcd
