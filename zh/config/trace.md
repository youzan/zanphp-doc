# trace.php

### 文件位置
```
resource/config/$ENV/monitor/trace.php
```

### 配置作用

调用链相关配置

### 配置文件内容

````php
<?php

return [
    //是否开启调用链上报
    //如果开启需要先配置trace所需的tcp连接池
    "run" => false,
];
````