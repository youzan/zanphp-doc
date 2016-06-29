# hawk.php

### 文件位置
```
resource/config/$ENV/hawk.php
```

### 配置作用

上报hawk监控所需数据的配置

### 配置文件内容

````php
<?php

return [
    //是否运行, pre不需要运行
    'run' => false,
    'host' => '1.1.1.1',
    'port' => 1234,
    'uri' => '/monitor/push',
    //多久上报一次，单位毫秒
    'time' => 60000,
];
````