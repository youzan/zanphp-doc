# 路由

zan框架的url组成规则为：

```php
scheme://host:port/route/controller/action?query
```

url组成中的path由route/controller/action组成，controller如需要访问src/Controller/Index/IndexController下的index方法，则对应的url为

```php
scheme://host:port/index/index/index?query
```

此外，zan框架设置了提供了设置默认路由的配置文件，路径为resource/config/share/route.php，各环境公用此配置文件。配置示例为：

```php
<?php

return [
    'default_route' => '/index',
    'default_controller' => 'index',
    'default_action' => 'index',
    'default_format' => 'html',
];
```

default\_format指定返回页面的content-type字段，default\_format与content-type之间的映射关系为：

```php
'html' => 'text/html',
'txt' => 'text/plain',
'js' => 'application/javascript',
'css' => 'text/css',
'json' => 'application/json',
'xml' => 'text/xml',
'rdf' => 'application/rdf+xml',
'atom' => 'application/atom+xml',
'rss' => 'application/rss+xml',
'form' => 'application/x-www-form-urlencoded'
```



