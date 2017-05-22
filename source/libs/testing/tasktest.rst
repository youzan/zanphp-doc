TaskTest
========

zan框架基于协程构建，具备异步无阻塞等特性，在测试用例上，zan框架提供了统一的基类TaskTest。

使用示例
~~~~~~~~

编写测试用例只需要继承TaskTest，提供对应的task
method即可轻松实现任务调度。

.. code:: php

    class YieldTaskTest extends TaskTest {
        private function simpleFunction()
        {
            return;
        }

        private function generator()
        {
            yield 3;
        }

        public function taskYield()
        {
            $a = (yield $this->simpleFunction());
            $this->assertEquals(NULL, $a, 'Yield simpleFunction return value test failed');

            $a = (yield $this->generator());
            $this->assertEquals(3, $a, 'Yield Generator test failed');
        }

    }

-  测试方法命名规则为task+name，测试方法为协程。
-  当有多个task异步操作时，调度表现为多IO并发形式，故存在依赖关系的task需写入一个task中。
