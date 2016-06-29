# hawk.php

### 文件位置
```
resource/config/$ENV/hawk.php
```

### 配置作用

上报hawk数据的配置

### 配置文件内容

````php
<?php

return [
    //是否运行, pre不需要运行
    'run' => false,
    'host' => '192.168.66.240',
    'port' => 8188,
    'uri' => '/monitor/push',
    //上报时间，单位毫秒
    'time' => 60000,
];
````