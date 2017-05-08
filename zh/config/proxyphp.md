# proxy.php

### 文件位置

```
resource/config/$ENV/proxy.php
```

### 配置作用

设置信任的server反向代理ip，只有从受信任的代理ip转发至server的包纳入真实客户端ip地址统计，防止ip地址欺骗。

### 配置文件内容

```php
<?php

return [
    "x.x.x.x",
    "a.a.a.a",
];
```



