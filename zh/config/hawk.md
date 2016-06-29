# hawk.php

### 文件位置
```
resource/config/$ENV/hawk.php
```

### 配置文件内容

````php
<?php

return [
    //是否运行, pre不需要运行
    'run' => false,
    'host' => '192.168.66.240',
    'port' => 8188,
    'uri' => '/monitor/push',
    //上报时间每60秒一次
    'time' => 60000,
];
````