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
$js->syncLoad($index, $vendor = false, $crossorigin = false);
$js->asyncLoad($index, $vendor = false);
```
###JsVar.php
负责后端跟前端通信，对数据进行了初步分组（详见注释）
``` php
$jsVar = new JsVar();
$jsVar->setSession($key, $value);
$jsVar->get();
```
##简单示例
