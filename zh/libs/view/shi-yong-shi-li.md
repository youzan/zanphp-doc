## 使用示例

### 文件目录

```php
├── Controller
│   ├── Index
│   │   └── IndexController.php
│   └── Test
│       └── ExceptionController.php
├── Demo
│   ├── Service
│   │   └── HttpCall.php
│   └── View
│       └── test
│           └── test.html
└── Model
    └── Index
        └── GetAllDemoData.php
```

### IndexController.php

```php
namespace Com\Youzan\ZanHttpDemo\Controller\Index;

use Com\Youzan\ZanHttpDemo\DemoModule\Service\System;
use Zan\Framework\Foundation\Domain\HttpController as Controller;

class IndexController extends Controller {
    public function index()
    {
        $systemService = new System(); 
        yield $systemService->sleep(20);
        $this->assign('testResult', 'hello, zanphp!');
        yield $this->display('DemoModule/Test');
    }
}
```

### Base.html

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
    <?php $layout->place('title'); ?>
    </title>
</head>
<body>
<?php $layout->place('body'); ?>
</body>
</html>
```

### Test.html

```php
<?php $layout->extend('DemoModule/Base'); ?>

<?php $layout->block('title'); ?>
Zanphp Demo
<?php $layout->endBlock();?>


<?php $layout->block('body'); ?>
<div class="test">
    <?php echo "Test Result: " . $testResult ?>
</div>
<?php $layout->endBlock();?>
```



