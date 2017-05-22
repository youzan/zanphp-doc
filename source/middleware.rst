Middleware
==========

约定
----

目录结构
~~~~~~~~

必须在resource/middleware/目录下的middleware.php中。 ###规则格式

.. code:: php

    return [
        'group'     => [
            'demo'   => [
                Com\Youzan\ZanHttpDemo\Middleware\DemoFilter::class,
                Com\Youzan\ZanHttpDemo\Middleware\DemoTerminator::class
            ],
        ],
        'match'     => [
            ['^route/.*', 'demo'],
        ],
    ];

其中group表示一个middleware分组，然后将此分组运用的下面待匹配的match中。match为一个二维数组，其中一个单元为包含2个item的数据，item1表示用来配置route的正则，item2表示起对应需要被运用到middleware
group。 ###简单示例 可参考zanhttp这个示例项目中的如下请求：
http://localhost:8030/route1\_test/param\_a\_value/route1
