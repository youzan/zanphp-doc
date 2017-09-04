Terminators
===========

功能
----

请求结束时的中间件，主要包括资源的回收和调用链信息上报等功能。

接口
----

.. code:: php

    interface RequestTerminator {
        /**
         * @param Request $request    请求实例
         * @param Response $response  响应实例
         * @param Context $context    请求上下文
         * @return void
         */
        public function terminate(Request $request, Response $response, Context $context);
    }

-  terminate方法的实现可以为协程或者纯函数。

配置请求与terminate对应关系的规则与Filter相同，类似为：

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
            [
                ".*", "all"
            ]
        ],
        'group' => [
            "genericServiceFilterGroup" => [
                TraceTerminator::class
            ],
            "all" => [
                TraceTerminator::class
            ],
        ]
    ];


示例
----

.. code:: php

    class TraceTerminator implements RequestTerminator
    {
        public function terminate(Request $request, Response $response, Context $context)
        {
            $trace = $context->get('trace');
            if (method_exists($response, 'getException')) {
                $exception = $response->getException();
                if ($exception) {
                    $trace->commit($exception);
                } else {
                    $trace->commit(Constant::SUCCESS);
                }
            } else {
                $trace->commit(Constant::SUCCESS);
            }

            //send数据
            yield $trace->send();
        }
    }

以TraceTerminator为例，其通过context获取trace实例之后，将监控信息进行上报。

自定义terminator会在框架terminator执行之前执行

