# Qrcode

zan框架屏蔽了二维码生成的细节，业务可以便捷使用接口生成二维码。

### 配置

配置文件路径：resource/config/$ENV/services/qrcode.php。

```php
<?php

return [
    //二维码生成的server端host和port
    'host' => 'qrcode-qa.s.qima-inc.com',
    'port' => 8888,
];
```

### 接口

```php
class Qrcode {
    public static function create($data,$size='200x200',$base64=false, $styles = []);
}
```

入口参数的含义为

* txt: 二维码内容
* size: 二维码大小
* base64: 是否进行base64编码
* styles：其他参数设定，具体参考[http://doc.qima-inc.com/pages/viewpage.action?pageId=4326255](http://doc.qima-inc.com/pages/viewpage.action?pageId=4326255)。

### 使用示例

```php
$text = "youzan";
$size = '270x270';
$qrCode = (yield Qrcode::create($text, $size, true));
```



