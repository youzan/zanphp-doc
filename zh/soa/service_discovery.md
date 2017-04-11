# 服务注册&发现&下线

## 1.服务注册

服务注册，是指nova服务启动后，注册到服务中心，告诉服务中心该服务可用。

流程：swoole\_server在master进程启动后，获取api提供的services与method，发送post请求注册到注册中心。目前所有的服务注册都是通过本地代理haunt连接etcd，注册成功的服务可以从etcd拉取注册信息，具体方法参见[http://doc.qima-inc.com/display/engineer/Etcd](http://doc.qima-inc.com/display/engineer/Etcd)。

### 1.1.配置

需要注册的服务位于[resource/config/$ENV/nova.php](/zh/config/nova.md)，目前所有的服务通过haunt注册，haunt的地址配置见[resource/config/$ENV/haunt.php](/zh/config/haunt.md)。

## 2.服务发现

服务发现，是nova服务启动后，从服务中心拉取需要的服务列表，并建立nova链接；监听服务中心，拉取服务列表的变化，若服务下线，则将对应的nova链接下线，若新服务上线，则建立对应的nova链接，供业务使用。

### 2.1.配置

服务发现直接从etcd拉取服务信息，具体配置为[resource/config/$ENV/haunt.php](/zh/config/haunt.md)，需要发现的服务会在程序启动时自动拉取服务信息。

### 2.2.本地开发配置

本地开发若不想走服务发现，直连本机的nova服务端，需要加入如下配置

新加在config下加入.private文件夹，修改.gitignore加入 .private/ 该目录不推送到git仓库中

在.private里加入service\_discovery.php文件

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

## 3.服务下线

### 3.1.配置

需要在PHP服务化业务应用根目录的composer.json中做如下改动：

```php
"zan-tools/publish":"dev-master",
```

在require中增加下面代码\(注意最后逗号是否需要加\)

```php
"scripts": {
    //如果是http server
    "post-update-cmd": "Com\\Youzan\\Tools\\Publish\\Http::postUpdate"
    //如果是tcp server
    "post-update-cmd": "Com\\Youzan\\Tools\\Publish\\Tcp::postUpdate"
}
```

执行composer update

查看根目录bin/publish 下面是否有生成出来的shell文件，如果有则表示正常，下次提交发布时可以将publish带上去一起发布。

### 3.2.下线

#### Tcp

```bash
 /home/www/%(appName)/bin/publish/offline.sh %(appName)
 /home/www/%(appName)/bin/publish/online.sh %(appName)
```

执行脚本完成后，重新拉取ectd信息即可确认服务是否已经下线。etcd各环境的域名参见[http://doc.qima-inc.com/display/engineer/Etcd](http://doc.qima-inc.com/display/engineer/Etcd)，拉取信息的url组成规则为

```
curl "http://{host}:{port}/v2/keys/{protocol}:{domain}/{appName}"
```

protocol、domain和appName为Nova服务发布时的配置项，具体见应用配置[nova.php](/zh/config/nova.md).

protocol默认值为nova，domain默认值为com.youzan.service，如

curl "http://xx.xx.xx.xx:xx/v2/keys/nova:com.youzan.service/scrm-customer-base"

#### Http

```
/home/www/%(appName)/bin/publish/offline.sh
/home/www/%(appName)/bin/publish/online.sh
```

http的服务上下线在nginx侧处理，具体参考文档[http://doc.qima-inc.com/pages/viewpage.action?pageId=11855950](http://doc.qima-inc.com/pages/viewpage.action?pageId=11855950)

