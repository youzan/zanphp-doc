#View
##约定
###目录结构
在src目录下，目前只支持module形式，并且视图文件必须位于module下的view中，如下所示：
``` php
├── Controller
│   └── Index
│       └── IndexController.php
└── DemoModule
    ├── Service
    │   └── System.php
    └── View
        ├── Base.html
        └── Test.html
```
###大小写
src下所有目录（包括文件）都采用大写字母开头，驼峰式的命名方式。
##涉及class及主要函数列表
###HttpController.php
web端控制器基类
``` php
$httpController = new HttpController(Request $request, Context $context);
//网模板中写入业务数据
$httpController->assign($key, $value);
//渲染模板
$httpController->display($tpl);
```
###View.php
对外暴露的显示模板文件的静态方法
``` php
View::display($tplPath, array $data = []);
```
###Tpl.php
加载模板，并将模板需要渲染的变量写入符号表
``` php
$tpl = new Tpl(Event $event);
$tpl->load($tpl, array $data = [])
```
###Css.php
加载css文件
``` php
$css = new Css(Event $event);
$css->load($index, $vendor = false)
```
###Js.php
加载js文件，分为同步和异步
``` php
$js = new Js(Event $event);
//同步加载
$js->syncLoad($index, $vendor = false, $crossorigin = false);
//异步加载
$js->asyncLoad($index, $vendor = false);
```
###JsVar.php
负责后端跟前端通信，对数据进行了初步分组（详见该类注释）
``` php
$jsVar = new JsVar();
$jsVar->setSession($key, $value);
$jsVar->get();
```
###Layout.php
布局类，将页面切分成各个block。
``` php
$layout = new Layout(Tpl $tpl, Event $event, $tplPath);
//开始一个block
$layout->block($blockName);
//闭合一个block，必须配合block使用
$layout->endBlock($blockName = null);
//继承一个页面
$layout->extend($tplPath);
//调用父类block
$layout->super($blockName = null);
//设置一个占位的block，一般用在父类页面，使其能被子类block替换
$layout->place($blockName);
```
##简单示例
###IndexController.php
``` php
use Com\Youzan\ZanHttpDemo\DemoModule\Service\System;
use Zan\Framework\Foundation\Domain\HttpController as Controller;

class IndexController extends Controller {
    public function index()
    {
        $systemService = new System(); 
        yield $systemService->sleep(20);
        $this->assign('welcome', 'Hello World!');
        yield $this->display('DemoModule/Test');
    }
}
```
###Base.html
``` php
<!DOCTYPE html>
<html class="<?php $layout->place('html_class'); ?>" lang="zh-CN" >
<head>
    <meta charset="utf-8">
    <meta name="HandheldFriendly" content="True">
    <meta name="MobileOptimized" content="320">
    <meta name="format-detection" content="telephone=no">
    <meta http-equiv="cleartype" content="on">

    <?php $layout->block('load_js');?>
    <?php $js->asyncLoad('async_a.js');?>
    <?php $js->asyncLoad('async_b.js');?>
    <?php $layout->endBlock();?>

    <?php $layout->block('meta_block'); ?>
    <?php $layout->endBlock(); ?>

    <?php $js->syncLoad('sync_a.js');?>
    <?php $js->syncLoad('sync_b.js');?>

    <?php $css->load('css_a.css');?>
    <?php $css->load('css_b.css');?>

    <?php $js->asyncLoad('async_c.js');?>

    <title>
        <?php $layout->place('title'); ?>
    </title>
</head>

<body>
    <div class="container <?php $layout->place('page-special-class'); ?>">
        <?php $layout->place('content'); ?>
    </div>

    <?php $layout->block('tpl_load');?>
    <?php $layout->endBlock();?>

    <?php $layout->block('footer'); ?>
    <?php $layout->endBlock();?>

    <?php $js->syncLoad('common_js');?>
</body>
</html>
```
###Content.html
``` php
<?php $layout->extend(__DIR__ . '/base.html'); ?>

<?php $layout->block('title'); ?>
<?php echo $title;?>
<?php $layout->endBlock();?>

<?php $layout->block('content'); ?>
<?php echo $content;?>
<?php $layout->endBlock();?>

<?php $layout->block('load_js');?>
<?php $js->asyncLoad('async_d.js');?>
<?php $js->asyncLoad('async_e.js');?>
<?php $layout->endBlock();?>

<?php $layout->block('load_js_b');?>
<?php $js->asyncLoad('async_d.js');?>
<?php $js->asyncLoad('async_e.js');?>
<?php $layout->endBlock();?>
```
