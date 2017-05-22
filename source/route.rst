Route
=====

约定
----

目录结构
~~~~~~~~

在resource/routing目录下，以.routing.php为后缀的文件中编写路由规则，可按module进行区分。
###规则格式

.. code:: php

    return [
        '^route1_test/:param_a_key/route1(/)?.*$' => 'route/route/route1',
    ];

其中数组的key用来匹配实际url的正则表达式，value为匹配成功后转发到的路由。需要特别说明的是，:param\_a\_key是会从url中提取出变量放入框架的request中。
###简单示例 可参考zanhttp这个示例项目中的如下请求：
http://localhost:8030/route1\_test/param\_a\_value/route1
