# Nova

Zan框架在进行服务发现，与其他服务采用Nova协议通信时，会从连接池中取出一条连接进行数据传输。

### 配置

Nova连接配置在项目 resource/config/$ENV/connection下。

```php
return [
    'engine'=> 'novaClient',
    //连接超时时间
    'connect_timeout' => 15000,
    //心跳时间
    'heartbeat-time' => 15000,
    //负载均衡策略,目前只支持polling
    'load_balancing_strategy' => 'polling',
    'config'    => [
        //swoole包头格式设置
        'open_length_check' => 1,
        'package_length_type' => 'N',
        'package_length_offset' => 0,
        'package_body_offset' => 0,
        //是否开启nova协议，1:开启，0:关闭
        'open_nova_protocol' => 1
    ],
];
```

不同于其他连接池，Nova连接池的连接数目、连接目的地址无法配置。主要原因是Nova连接池的目标地址为服务提供方的地址，服务提供方的信息来源于注册中心。另外，每一个外部服务在zan框架中对应一个Nova连接池，Nova连接池中的连接数目等于提供外部服务的server数目。

