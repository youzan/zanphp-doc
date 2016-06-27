# 服务发现
> 服务发现主要包含了服务注册、服务发现两部分。

## 服务注册
服务注册，是指nova服务启动后，注册到服务中心，告诉服务中心该服务可用。

流程：swoole_server在master进程启动后，获取api提供的services与method，发送post请求注册到注册中心。

## 服务发现
服务发现，是nova服务启动后，从服务中心拉取需要的服务列表，并建立nova链接；监听服务中心，拉取服务列表的变化，若服务下线，则将对应的nova链接下线，若新服务上线，则建立对应的nova链接，供业务使用。

## 配置
依赖php apcu扩展，yz_swoole扩展版本要在1.0.4以上

在 resource/config/{$ENV}/haunt.php 里加入配置文件 
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

## 本地开发配置
本地开发若不想走服务发现，直连本机的nova服务端，需要加入如下配置

新加在config下加入.private文件夹，修改.gitignore加入 .private/ 该目录不推送到git仓库中

在.private里加入service_discovery.php文件


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



