# 目录结构

在src目录下，目前只支持module形式，并且视图文件必须位于module下的view中，否则会导致模板加载出错。module下通常包含view和service，view下存放前端页面模板，service存放调用其他服务的方法。Model目录存放数据库的操作，zan框架的数据库操作主要采用sqlMap的方式生成sql语句，sql语句的模板位于resource/sql 目录下。Controller目录下设子目录和对应的controller文件，controller内实现对应的method方法，如下所示：

```php
├── Controller
│   ├── Index
│   │   └── IndexController.php
│   └── Test
│       └── ExceptionController.php
├── Demo
│   ├── Service
│   │   └── HttpCall.php
│   └── View
│       └── test
│           └── test.html
└── Model
    └── Index
        └── GetAllDemoData.php
```

# 命名规范

src下所有目录（包括文件）都采用大写字母开头，驼峰式的命名方式。

Controller子目录下文件名必须以Controller.php结尾。

