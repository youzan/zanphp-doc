# Log

目前支持的日志类型有：`file`（绝对路径）、`log`（项目相对路径，会写到项目的resource/log目录下）、`syslog`（flume log系统，内部系统，暂无开源计划，如需使用需自行开发）、`blackhole`（黑洞）。
file、log、syslog类型都支持buffer，可以在 log.php 配置 useBuffer=true 启用。

## 配置
要在你的项目中使用前，你需要进行一些配置：
请在 resource/config/ 里添加各个环境需要的配置（log.php）
如 test 环境的 resource/config/test/log.php 

```PHP
<?php
 
return [
    'debug'          => 'log://info/debug.log?useBuffer=true&format=json',
    'file_test'      => 'file://warning/file_test.log?format=var',
    'blackhole_test' => 'blackhole://warning/',
    'syslog_test'    => 'syslog://error/syslog_test?storeType=normal',
];
```
以上面的debug配置为例：log://all/debug.log 解析之后：

`scheme`为 log 代表日志类型；

`host` 为 info 表示日志级别；

`path` 为 debug.log 表示 写入log的文件名（最终实际路径为项目的resource/log/debug.log）；

`query` 为 useBuffer=true&format=json 再次解析之后表示 启用buffer 和 格式化为json的形式存入log文件。


## API
Log是一个工厂类，获取到的日志实例类实现了 Psr\Log\LoggerInterface 接口，有以下方法：emergency, alert, critical, error, warning, notice, info, debug, log。
其中log方法是通过传入level的形式调用error, warning, notice等方法。
```PHP

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

```

## 示例

```PHP
// 普通日志参数
yield \Log::make('debug')->info('Hello Log!');
 
// 带exception的日志参数
yield \Log::make('trade')->error('I am a exception!', [
    'exception' => new InvalidArgumentException('Nickname shoud be a string !'),
    'other1' => 123,
    'other2' => 'abc',
]);
```


