# MQ

zan框架MQ采用go语言开源消息队列NSQ，提供操作NSQ的客户端SDK，对应的包名称为nsq-client。

### 配置

MQ连接配置在项目 resource/config/$ENV/nsq.php下。

```php
<?php
/**
 * 说明:
 * 1. 只有lookup项必填, 其他全部选填
 * 2. 所有时间配置 单位: ms
 */
return [
    // ["必填"]lookup 节点地址
    "lookup" => [
        "http://sqs-qa.s.qima-inc.com:4161"
    ]
];
```

另外，workerStart文件夹下需要新增配置文件.config.php

```php
<?php
use Zan\Framework\Components\Nsq\InitializeSQS;

return [
    InitializeSQS::class,
];
```

### 接口

NSQ目前提供pub和sub接口，接口规范为

```php
<?php
class SQS {
    /**
     * 订阅
     * @param string $topic
     * @param string $channel
     * @param MsgHandler|callable $msgHandler
     * @param int $maxInFlight
     * @return \Generator yield return Consumer
     * @throws NsqException
     */
    public static function subscribe($topic, $channel, $msgHandler, $maxInFlight = -1);

    /**
     * 取消订阅
     * @param string $topic
     * @param string $channel
     * @return bool
     */
    public static function unSubscribe($topic, $channel);

    /**
     * 发布
     * @param string $topic
     * @param string[] ...$messages
     * @return \Generator yield bool
     * @throws NsqException
     */
    public static function publish($topic, ...$messages);

    /**
     * 统计信息
     * @return array
     */    
    public static function stat();
}
```

### 使用示例

* #### 订阅

```php
$topic = "zan_mqworker_test";
$ch = "ch";
//msgHandler为callable function
yield SQS::subscribe($topic, $ch, function(Message $msg, Consumer $consumer) {});
//msgHandler为interface MsgHandler
yield SQS::subscribe($topic, $ch, new BenchMsgHandler(), 1);
```

* #### 取消订阅

```php
yield SQS::unSubscribe($topic, $ch);
```

* #### 发布

```php
$oneMsg = "hello";
$multiMsgs = [
    "hello",
    "hi",
];
yield SQS::publish($topic, $oneMsg);
yield SQS::publish($topic, "hello", "hi");
yield SQS::publish($topic, ...$multiMsgs);
```



