# 项目配置

### 一、zan框架配置

#### 目录结构

```
resource
├── cache
├── config
├── middleware
├── routing
└── sql
```

zan框架的配置文件位于$ROOTPATH/resource文件夹下，各子目录存放内容为：

cache：redis中的key模板配置

config：各环境关于连接池、服务发现等配置的文件夹

middleware：请求过滤和异常处理中间件的匹配规则

sql：sql语句模板配置，使用时传入data即可组成sql语句

各环境配置文件都放在$ROOTPATH/resource/config/$ENV 文件夹下，下面以test环境举例，结构如下：

```
resource/config/test/
├── connection
│   ├── kvstore.php
│   ├── mysql.php
│   └── tcp.php
├── debug.php
├── haunt.php
├── hawk.php
├── monitor
│   └── trace.php
└── server.php
```

$ROOTPATH/resource/config/share文件夹为所有环境共享配置，可用于存放业务相关，环境无关的配置。

### 二、应用初始化配置

```
├── app.php
├── ServerStart
│   ├── .config.php
│   └── LoadUrlConfig.php
└── WorkerStart
    ├── .config.php
    └── LoadUrlConfig.php
```

除了自身的配置之外，zan框架另外提供了应用自定义初始化配置的钩子，配置文件位于init文件夹下，名称为.config.php

ServerStart和WorkerStart文件夹分别用于存放server进程和worker进程启动时的初始化操作，文件示例为

.config.php

```php
<?php

return [
    \Scrm\Open\Init\WorkerStart\LoadUrlConfig::class,
];
```

LoadUrlConfig.php

```php
<?php
namespace Scrm\Open\Init\WorkerStart;

use Zan\Framework\Contract\Network\Bootable;
use Zan\Framework\Foundation\Core\Config;
use Zan\Framework\Utilities\Types\URL;

class LoadUrlConfig implements Bootable
{
    public function bootstrap($server)
    {
        $config = Config::get('url');
        URL::setConfig($config);
    }
}
```

应用可根据需要实现Zan\Framework\Contract\Network\Bootable接口，然后将类名注册到.config.php文件的返回数组即可。

