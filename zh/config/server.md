# server.php

### 文件位置
```
resource/config/$ENV/nova.php
```

### 配置作用

注册Nova服务配置

### 配置文件内容

````php
<?php

return [
    'novaApi' => [
        'path'  => 'vendor/zanphp/novatcpdemo/gen-php',
        'namespace' => 'Com\\Youzan\\NovaTcpDemo\\',
    ],
];
````