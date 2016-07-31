# Service Discovery
Service discovery includes service registration and discovery.

## Service Registration
Service Registration. The process of a nova service registering it’s location in a central registry,and to be useful status.

Process：swoole_server started in master process，get services and method from api, then post request registration to central registry.

## Service Discovery
Service Discovery.after a nova service started,the process of a client application querying the central registry to learn  of the location of services,then create a nova connection.
And listen service central registry,pull changed service list.If a service down,then the connected nova service down.

## Config
Dependence extension: php apcu，yz_swoole extension: above 1.0.4

resource/config/{$ENV}/haunt.php Add Config File:
```php
<?php
return [
    //拉取需要的服务列表，此处填写注册到注册中心的的app name，如果无需拉去任何服务，app_names为空array即可
    'app_names' => [
        'xxx-api','xxx1-api'
    ],
    //拉取服务配置 固定配置，业务无需修改
    'discovery' => [
        'host' => '127.0.0.1', 
        'port' => 9000,
        'timeout' => 30000,
        'uri' => 'uri',
        'protocol' => 'nova',
        'namespace' => 'com.youzan.service', //固定配置与业务无关，下面配置同理
        'loop_time' => 1000,     //worker定时器任务执行时间（判断是否已拉取到服务）
    ],
    //监听服务配置 固定配置，业务无需修改
    'watch' => [
        'host' => '127.0.0.1',
        'port' => 9000,
        'timeout' => 30000,
        'uri' => 'uri',
        'protocol' => 'nova',
        'namespace' => 'com.youzan.service',
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
        'namespace' => 'com.youzan.service',
        'enable_register' => 1, //此处新加，0则不注册，1为注册，可以不填enable_register这个key，框架会默认注册
    ],
];
```

## Local Development Config
Local Development common connects the nova server,no service registration.

1. add folder .private in config directory , add .private/ to .gitignore file, make git ignored.
2. add service_discovery.php to .private


```php
<?php
return [
    //不需要服务发现app_names
    'app_names' => [
        'xxx-api',
    ],
    //不需要服务发现app的novaApi配置
    'novaApi' => [
        'xxx-api' => [//注意此处key为不需要服务发现app_names
            'path'  => 'vendor/nova-service/xxx-api/gen-php',
            'namespace' => 'Com\\Youzan\\Xx\\',
        ],
    ],
    //直连的app host port
    'connection' => [
        'xxx-api' => [//注意此处key为不需要服务发现app_names
            'host' => '127.0.0.1',
            'port' => '9000',
        ],
    ],
];
```



