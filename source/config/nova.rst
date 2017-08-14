nova.php
========

文件位置
~~~~~~~~

::

    resource/config/$ENV/nova.php

配置作用
~~~~~~~~

注册Nova服务配置

配置文件内容（旧配置，建议使用新的\ `registry <../../html/config/registry.html>`__\配置）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

单组nova api配置

.. code:: php

    <?php

    return [
        'novaApi' => [
            //nova-service的vendor目录，直到gen-php
            'path'  => 'vendor/zanphp/novatcpdemo/gen-php',
            //gen-php文件夹对应的namespace
            'namespace' => 'Com\\Yourcompany\\NovaTcpDemo\\',
        ],
    ];

多组nova api配置

.. code:: php

    <?php

    return [
        'novaApi' => [
            [
                'path'  => 'vendor/nova-service/scrm-base/gen-php',
                'namespace' => 'Com\\Yourcompany\\Scrm\\',
                'domain' => 'com.example.service', // 可选, 默认 com.example.service, 配置服务发布到 具体的域
                'appName'   => 'scrm', // 可选, 默认Application::getName(), 配置服务发布的 应用名
                'protocol'   => 'nova', // 可选, 目前恒等于 nova
            ],
            [
                'path'  => 'vendor/nova-service/scrm-core/gen-php',
                'namespace' => 'Com\\Yourcompany\\Scrm\\',
                'domain' => 'com.example.service', // 可选, 默认 com.example.service, 配置服务发布到 具体的域
                'appName'   => 'scrm', // 可选, 默认Application::getName(), 配置服务发布的 应用名
                'protocol'   => 'nova', // 可选, 目前恒等于 nova
            ],
        ],
    ];

注意
''''

-  同一protocol和appName的 namespace需要相同

