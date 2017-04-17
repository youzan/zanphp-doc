# 路由

zan框架的url组成规则为：

```php
scheme://host:port/route/controller/action?query
```

url组成中的path由route/controller/action组成，controller如需要访问src/Controller/Index/IndexController下的index方法，则对应的url为

```php
scheme://host:port/index/index/index?query
```

此外，zan框架设置了提供了设置默认路由的配置文件，路径为resource/config/share/route.php

