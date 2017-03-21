# Barcode

zan框架屏蔽了条形码生成的细节，业务可以便捷使用接口生成条形码。

### 配置

配置文件路径：resource/config/$ENV/services/barcode.php。

```php
<?php

return [
    //条形码生成的server端host和port
    'host' => 'barcode-qa.s.qima-inc.com',
    'port' => 8888,
    //请求超时时间
    'timeout' => 3000,
];
```

### 接口

```php
class Barcode {
    public static function create($text, $height = 10, $styles = [], $barcode = 20);
    public static function createDataUrl($text, $height = 10, $styles = [], $barcode = 20)
}
```

入口参数的含义为

* txt: 条形码内容
* height: 条形码高度
* styles：其他参数设定，如
  * bg: 背景色 6位16进制 000000-ffffff
  * fg: 前景色 6位16进制 000000-ffffff
  * roate: 旋转角度 只能为［0，90，180，270］
  * scale：放大缩小倍数 ［0.01-3］
  * hrt：不显示一维码下标，无论输入什么，只有有这个参数，就不显示下标
* barcode: 条形码类型

create和createDataUrl方法的区别在于create返回原始数据，createDataUrl返回base64编码后数据

### 使用示例

```php
$text = "youzan";
$height = 10;
$styles = [
    'rotate' => 0,
    'scale' => 1,
    'bg' => 'ffffff',
    'fg' => '000000',
    'hrt' => 1
];
$barcode = 20;

$result = (yield Barcode::createDataUrl($text, $height, $styles, $barcode));
```



