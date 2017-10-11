项目目录结构
============

基本项目目录如下:

::

    ├── bin
    ├── init
    │   ├── ServerStart
    │   └── WorkerStart
    ├── public
    ├── resource
    │   ├── cache
    │   ├── kvstore
    │   ├── config
    │   │   ├── qatest
    │   │   │   ├── connection
    │   │   │   └── ...
    │   │   ├── online
    │   │   │   ├── connection
    │   │   │   └── ...
    │   │   ├── share
    │   │   │   └── table
    │   │   └── test
    │   │       ├── connection
    │   │       └── ...
    │   └── sql
    │       ├── common
    │       └── ...
    ├── src
    │   ├── Controller(http server)
    │   ├── Bo
    │   ├── Dao
    │   └── Service
    ├── tests
    │   ├── Bo
    │   ├── Dao
    │   └── Service
    └── vendor
        └── ...

目录说明
~~~~~~~~

-  bin: 服务启动bin文件目录
-  init: 应用初始化相关
-  resource: 配置文件目录，具体配置见 `项目配置 <../config/index.html>`__
-  src: 逻辑代码目录
-  tests: 单元测试目录
-  vendor: composer引入包目录
