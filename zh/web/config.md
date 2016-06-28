# 项目配置

正常配置文件都放在$ROOTPATH/resource/config/$ENV 文件加下，结构如下：

````
resource/
├── cache
├── config
│   ├── online
│   │   └── connection
│   ├── qatest
│   │   └── connection
│   ├── share
│   │   └── table
│   └── test
│       ├── connection
│       ├── kvstore
│       │   └── test
│       └── monitor
├── kvstore
└── sql
````

### 目录说明
* cache: cache的配置目录，详情见 [Redis](libs/nosql/redis.md)
