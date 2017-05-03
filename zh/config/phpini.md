# php.ini配置

执行命令获取php.ini文件位置

```
php -i|grep php.ini
```

在php.ini文件末尾添加

```php
date.timezone = "Asia/Chongqing"
kdt.RUN_MODE = test
kdt.DEBUG = true
```

* date.timezone：设置时间时区

* kdt.RUN\_MODE：程序运行模式，对应resource/config下的子目录，如kdt.RUN\_MODE = test对应配置resource/config/test生效，默认值为online
* kdt.DEBUG：是否开启调试模式，默认值为false



