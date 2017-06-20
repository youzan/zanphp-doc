路由
====

zanphp框架的url组成规则为：

.. code:: php

    scheme://host:port/route/controller/action?query

url组成中的path由route/controller/action组成，controller如需要访问src/Controller/Index/IndexController下的index方法，则对应的url为

.. code:: php

    scheme://host:port/index/index/index?query

此外，zan框架设置了提供了设置默认路由的配置文件，路径为resource/config/share/route.php，各环境公用此配置文件。配置示例为：

.. code:: php

    <?php

    return [
        'default_route' => '/index',
        'default_controller' => 'index',
        'default_action' => 'index',
        'default_format' => 'html',
    ];

default\_format指定返回页面的content-type字段，default\_format与content-type之间的映射关系为：

.. code:: php

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

自定义路由
----------
为满足不同业务开发的路由需求，zanphp框架支持自定义路由，只需要实现Zan\\Framework\\Network\\Http\\\Routing\\IRouter接口即可快速实现。

.. code:: php

    interface IRouter
    {
        /*
         * @return array ['module', 'controller', 'action']
         */
        public function dispatch(Request $request);
    }

为了与现有的路由文件组织形式保持一致，自定义路由需要返回Module-Controller-Action用于定位路由文件，其中，module/controller确定Controller文件位置，action为controller的方法。
业务可以结合自身需要将Restful Router、FastRouter等集成至zan框架中。

自定义实现的路由需要配置至框架中，修改resource/config/share/route.php配置文件，增加router_class选项，配置示例为：

.. code:: php

    <?php

    return [
        'default_route' => '/index',
        'default_controller' => 'index',
        'default_action' => 'index',
        'default_format' => 'html',
        'router_class' => '/namespace/router_class'  //完整的类名称
    ];

