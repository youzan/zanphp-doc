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

目前Terminator的自定义实现可以通过在InitializeMiddleware类中的$zanTerminators成员添加。Http和Tcp的InitializeMiddleware类文件位置分别为src/Network/Http/ServerStart/InitializeMiddleware.php和src/Network/Tcp/ServerStart/InitializeMiddleware.php。

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
