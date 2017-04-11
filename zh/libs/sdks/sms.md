# SMS

zan框架提供短信发送的SDK，业务方可以发送自定义短信内容。

### 配置

短信的配置位于vendor/zan-config/zan/src/ApiConfig.php文件中，配置结构类似于：

```php
'courier' => [
    'type' => 'php',
    'host' => 'http://xx.xx.xx.xx'
]
```

### 接口

```php
class SmsService {
   /**
     * @param MessageContext $messageContext
     * @param Recipient[]    $recipients
     *
     * @return bool
     */
    public function send(MessageContext $messageContext, array $recipients)
}
```

$messageContext实例包含短信模板名和参数，使用前需要在短信平台配置短信模板和参数规范。

$recipientsp配置短信接收人和发送人信息。

### 使用示例

```php
$param = array(
    'goodsName' => '饮料',
    'realPay' => '1.5',
    'link' => "http://www.youzan.com"
);
yield SmsService::getInstance()->send(
    new MessageContext('virtualPaySuc', $param),
    [new Recipient(Channel::SMS, 123456789)]   //接收人电话号码为123456789
);
```



