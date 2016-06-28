# 项目目录结构


基本项目目录如下: 

````
├── bin
├── init
│   ├── ServerStart
│   └── WorkerStart
├── public
├── resource
│   ├── cache
│   ├── kvstore
│   ├── routing
│   ├── config
│   │   ├── qatest
│   │   │   ├── connection
│   │   │   └── ...
│   │   ├── online
│   │   │   ├── connection
│   │   │   └── ...
│   │   ├── share
│   │   │   └── table
│   │   └── test
│   │       ├── connection
│   │       └── ...
│   └── sql
│       ├── common
│       └── ...
├── src
│   ├── Controller(http server)
│   ├── Bo
│   ├── Dao
│   └── Service
├── tests
│   ├── Bo
│   ├── Dao
│   └── Service
└── vendor
    └── ...
````

### 目录说明
* bin: 服务启动bin文件目录
* init: 应用初始化相关
* resource: 配置文件目录
