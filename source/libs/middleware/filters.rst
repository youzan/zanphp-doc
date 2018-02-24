Filters
=======

文件位置
~~~~~~~~

::

    resource/middleware/middleware.php

功能
----

自定义请求的过滤处理函数

配置示例
--------

tcp
~~~

.. code:: php

    <?php
    return [
        'match' => [
            // 可以单独针对 所有泛化调用 设置过滤器
            [
                "/com/Yourcompany/nova/framework/generic/service/GenericService/invoke", "genericServiceFilterGroup",
            ],
            // 也可以直接 针对特定服务配置, 在过滤器内检测是否是泛化调用
            [
                "/Com/Yourcompany/Nova/Framework/Generic/Php/Service/GenericTestService/ThrowException", "genericServiceFilterGroup",
            ],
            // 支持设置多个过滤器
            [
                "/com/Yourcompany/nova/framework/generic/service/GenericService/invoke", ["genericServiceFilterGroup", "another"],
            ],
            [
                ".*", "all"
            ]
        ],
        'group' => [
            "genericServiceFilterGroup" => [
                genericServiceFilter::class
            ],
            "another" => [
                anotherFilter::class
            ],
            "all" => [
                genericServiceFilter::class
            ],
        ]
    ];

-  match用于服务分组，服务名称支持正则表达式匹配。
-  group设置分组名和对应的请求过滤器，请求过滤器需要实现接口Zan\Framework\Contract\Network\RequestFilter，一个分组名可以设置多个处理器。

请求过滤器的实现示例为：

.. code:: php

    <?php
    use Zan\Framework\Contract\Network\RequestFilter;

    class genericServiceFilter implements RequestFilter
    {
        /**
         * @param \Zan\Framework\Contract\Network\Request $request
         * @param \Zan\Framework\Utilities\DesignPattern\Context $context
         * @return \Zan\Framework\Contract\Network\Response
         */
        public function doFilter(\Zan\Framework\Contract\Network\Request $request, \Zan\Framework\Utilities\DesignPattern\Context $context)
        {
            if ($request instanceof \Zan\Framework\Network\Tcp\Request) {
                //判断是否为泛化调用的请求
                if ($request->isGenericInvoke()) {
                    $route = $request->getRoute();
                    $rpcCtx = $request->getRpcContext();

                    // 两种方式获取透传参数
                    $id = $context->get("id", -1);

                    $id = $rpcCtx->get($id, 0);
                    if ($id === 42) {
                        // 抛出异常, 或者错误信息
                        throw new \RuntimeException("invalid id", 500);
                        //yield "invalid id";
                        return;
                    }
                }
            }
            yield null;
        }

    }

**doFilter对请求进行拦截处理，需要过滤掉的请求可以抛出异常信息或者yield返回响应消息，需要继续处理的请求yield
null，或者直接return即可。**

http
~~~~

http配置内容与tcp类似，不同点在于tcp以服务名和方法名为key，http以url的path为key（path未指明默认为index/index/index）来匹配分组，http示例为：

.. code:: php

    <?php
    use Zan\Framework\Network\Server\Middleware\TraceFilter;

    return [
        'group'     => [
            'all' => [
                TraceFilter::class
            ]
        ],
        'match'     => [
            ['market\/.*/', 'acl'],
            ['goods\/.*/', 'acl'],
            ['shop\/.*/', 'acl'],
            ['.*', 'all']
        ],
    ];

TraceFilter位于src/Network/Server/Middleware/TraceFilter.php文件。


自定义filter在框架filter之后执行
