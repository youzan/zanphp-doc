Logging
=======

目前支持的日志类型有：\ ``file``\ （绝对路径）、\ ``log``\ （项目相对路径，会写到项目的resource/log目录下）、\ ``syslog``\ （flume 
log系统，内部系统，暂无开源计划，如需使用需自行开发）、\ ``blackhole``\ （黑洞）。
file、log、syslog类型都支持buffer，可以在 log.php 配置 useBuffer=true
启用。
bufferSize默认值为 4096。

**useBuffer 对于性能有较大提升，如果业务允许，请尽量启用。**

配置
----

日志的配置文件为\ `resource/config/$ENV/log.php。 <../../config/log.html>`__

API
---

Log是一个工厂类，获取到的日志实例类实现了Psr\Log\LoggerInterface接口，有以下方法：emergency, alert, critical, error, warning, notice, info, debug, log。

其中log方法是通过传入level的形式调用error, warning, notice等方法。

.. code:: php

        /**
         * 系统不可用，彻底扑街了
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function emergency($message, array $context = array());

        /**
         * 必须立即采取措施
         *
         * 例如：整个网站的故障，数据库不可用等等。
         * 这应该触发短信提醒叫醒你。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function alert($message, array $context = array());

        /**
         * 危急情况
         *
         * 例如：应用程序组件不可用时，意外的异常。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function critical($message, array $context = array());

        /**
         * 不需要立即采取行动，但运行时错误通常应该被记录和监控。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function error($message, array $context = array());

        /**
         * 个别的事件并非错误。
         *
         * 例如：不赞成使用的API，使用不当的API的，不良的东西，并不一定是错的。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function warning($message, array $context = array());

        /**
         * 正常但是有意义的事件。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function notice($message, array $context = array());

        /**
         * 有趣的事件
         *
         * 例如：用户登录时，SQL日志。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function info($message, array $context = array());

        /**
         * 详细的调试信息。
         *
         * @param string $message
         * @param array $context
         * @return null
         */
        public function debug($message, array $context = array());

        /**
         * 可以使用任意级别的日志方法
         *
         * @param mixed $level
         * @param string $message
         * @param array $context
         * @return null
         */
        public function log($level, $message, array $context = array());

使用示例
--------

.. code:: php

    // 普通日志参数
    yield \Log::make('debug')->info('Hello Log!');

    // 带exception的日志参数
    yield \Log::make('trade')->error('I am a exception!', [
        'exception' => new InvalidArgumentException('Nickname shoud be a string !'),
        'other1' => 123,
        'other2' => 'abc',
    ]);
