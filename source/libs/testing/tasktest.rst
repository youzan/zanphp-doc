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

测试方法
~~~~~~~~
zan框架基于phpunit构建协程的统一测试用例，phpunit的使用方法详细参见\ `https://phpunit.de/ <https://phpunit.de/>`__

针对单个测试文件，测试命令为：

phpunit --bootstrap zan/test/bootstrap.php  [test.php]

如果需要运行zan框架全部的测试用例，需要搭建测试环境，包括tcp server，redis server，mysql server等。

tcp server和http server测试用例均已经模拟实现，文件位置为zan/test/mockServer。

redis server和mysql server需要用户本地搭建，然后修改zan/test/resource/config/{ENV}/connection下的连接池配置即可。

具体测试步骤为:

一、运行环境准备
>>>>>>>>>>>>>>>>>>

1.mysql server

2.redis server

修改test/resource/config/{ENV}/connection下的mysql用户名和密码

二、启动server
>>>>>>>>>>>>>>>>>>

除mysql和redis之外的其他server采用mock方式实现,启动方法:

cd zan/mockServer

sh ./go.sh start  启动server

三、执行测试
>>>>>>>>>>>>>>>>>>
cd zan

phpunit

四、关闭server
>>>>>>>>>>>>>>>>>>

cd zan/mockServer

sh ./go.sh stop   关闭server