如何使用ZanPHP TcpServer 提供Nova服务
=====================================

1. 编写nova服务thrift文件
2. 生成nova存根代码, 发布到仓库
3. tcp(nova服务端)项目 与 http(nova客户端)项目 composer.json 分别添加
   nova存根代码 依赖
4. tcp(nova服务端)项目 实现 nova 服务接口, 配置发布服务
5. http(nova客户端)项目 配置直接tcp-server，通过 nova
   存根代码调用nova服务

编写nova服务thrift文件
----------------------

thrift 结构

::

    .
    └── thrifts
        ├── entity
        │   ├── BaseStruct.thrift
        │   ├── ErrorLevel.thrift
        │   └── MixedStruct.thrift
        ├── exception
        │   └── DemoServiceException.thrift
        └── service
            └── DemoService.thrift

BaseStruct.thrift

::

    namespace nova com.yourcompany.demo.entity

    include 'ErrorLevel.thrift'

    struct BaseStruct {
        1:optional bool propBool,
        2:optional byte propByte,
        3:optional i16 propI16,
        4:optional i32 propI32,
        5:optional i64 propI64,
        6:optional double propDouble,
        7:optional string propString,
        8:optional ErrorLevel.ErrorLevel errorLevel
    }

ErrorLevel.thrift

::

    namespace nova com.yourcompany.demo.entity

    enum ErrorLevel {
        DEBUG = 1,
        INFO = 2,
        WARN = 3,
        ERROR = 4
    }

MixedStruct.thrift

::

    namespace nova com.yourcompany.demo.entity

    include 'BaseStruct.thrift'

    struct MixedStruct {
        1:optional string propString,
        2:optional BaseStruct.BaseStruct baseStruct,
        3:optional list<BaseStruct.BaseStruct> propList,
        4:optional set<BaseStruct.BaseStruct> propSet,
        5:optional map<string, BaseStruct.BaseStruct> propMap
    }

DemoServiceException.thrift

::

    namespace nova com.yourcompany.demo.exception

    exception DemoServiceException {
        1: string message
        2: i32 code
    }

DemoService.thrift

::

    namespace nova com.yourcompany.demo.service

    include '../entity/ErrorLevel.thrift'
    include '../entity/BaseStruct.thrift'
    include '../entity/MixedStruct.thrift'
    include '../exception/DemoServiceException.thrift'

    service DemoService {
        void    throwException() throws (1:DemoServiceException.DemoServiceException e);

        void    returnVoid();

        bool    returnBool();
        i32     returnI32();
        double  returnDouble();
        string  returnString();
        ErrorLevel.ErrorLevel returnEnum();


        BaseStruct.BaseStruct returnBaseStruct();
        MixedStruct.MixedStruct returnMixedStruct();
        list<BaseStruct.BaseStruct> returnList();
        set<BaseStruct.BaseStruct> returnSet();
        map<string, BaseStruct.BaseStruct> returnMap();

        void   paraBaseNoReturn(1:string paraString, 2:ErrorLevel.ErrorLevel errorLevel);

        void   paraMixedNoReturn (
            1:bool paraBool,
            2:i32 paraI32,
            3:double paraDouble,
            4:string paraString,
            5:BaseStruct.BaseStruct baseStruct,
            6:list<BaseStruct.BaseStruct> returnList,
            7:set<BaseStruct.BaseStruct> returnSet,
            8:map<string, BaseStruct.BaseStruct> returnMap,
            9:ErrorLevel.ErrorLevel errorLevel
        );

        map<string, BaseStruct.BaseStruct> complexMethod(
            1:bool paraBool,
            2:i32 paraI32,
            3:double paraDouble,
            4:string paraString,
            5:BaseStruct.BaseStruct baseStruct,
            6:list<BaseStruct.BaseStruct> returnList,
            7:set<BaseStruct.BaseStruct> returnSet,
            8:map<string, BaseStruct.BaseStruct> returnMap,
            9:ErrorLevel.ErrorLevel errorLevel
        )
    }

生成nova存根代码, 发布到仓库
----------------------------

下载或者编译zan-thrift工具
~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    cd thrifts
    zan-thrift --gen php # 执行工具生成存根代码

执行结果：

::

    Processing: .../nova-demo/thrifts/entity/BaseStruct.thrift
    Processing: .../nova-demo/thrifts/entity/ErrorLevel.thrift
    Processing: .../nova-demo/thrifts/entity/MixedStruct.thrift
    Processing: .../nova-demo/thrifts/exception/DemoServiceException.thrift
    Processing: .../nova-demo/thrifts/service/DemoService.thrift

tree

::

    ├── sdk
    │   └── gen-php
    │       ├── Entity
    │       │   ├── BaseStruct.php
    │       │   ├── ErrorLevel.php
    │       │   └── MixedStruct.php
    │       ├── Exception
    │       │   └── DemoServiceException.php
    │       ├── Interfaces
    │       │   └── DemoService.php
    │       ├── Service
    │       │   └── DemoService.php
    │       └── Servicespecification
    │           └── DemoService.php

编写composer.json
~~~~~~~~~~~~~~~~~

::

    {
        "name": "nova-service/nova-demo",
        "repositories": [
        ],
        "require": {
            "packaged/thrift": "0.9.2.1"
        },
        "autoload": {
            "psr-4": {
                "Com\\Yourcompany\\Demo\\": "sdk/gen-php"
            },
            "classmap": []
        }
    }

**注意 psr-4 命名空间要与thrift文件命名空间一致，遵守
com.\ :math:`{company}.`\ {module}[...]规范**

将生成的存根代码push到git仓库
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

tcp(nova服务端)项目 与 http(nova客户端)项目 composer.json 分别添加 nova存根代码 依赖
------------------------------------------------------------------------------------

::

    {
        ...
        "repositories": {
            # 这里因为测试demo的缘故，存根代码放在本地，通过路径引用，composer会在vendor中建立软连接
            # 正常项目引用独立的nova-service package
            {
                "type": "path",
                "url": "nova-sdk/nova-demo/",
                "options": {
                    "symlink": true
                }
            }
        },
        "require": {
            ...
            # require 添加
            "nova-service/nova-demo": "*"
        }
        ...
    }

tcp(nova服务端)项目 实现 nova 服务接口, 配置发布服务
----------------------------------------------------

tcp-demo/src/Service/DemoService.php

.. code:: php

    <?php

    namespace Com\Youzan\TcpDemo\Service;


    use Com\Yourcompany\Demo\Entity\BaseStruct;
    use Com\Yourcompany\Demo\Entity\ErrorLevel;
    use Com\Yourcompany\Demo\Entity\MixedStruct;
    use Com\Yourcompany\Demo\Exception\DemoServiceException;

    class DemoService implements \Com\Yourcompany\Demo\Interfaces\DemoService
    {


        public function returnBool()
        {
            yield true;
        }

        public function returnI32()
        {
            yield rand() % 1024;
        }

        public function returnDouble()
        {
            yield rand(1, 100) / (double)100;
        }

        public function returnString()
        {
            yield str_repeat('a', rand(1, 20));
        }

        public function returnEnum()
        {
            yield ErrorLevel::INFO;
        }

        ......
    }

配置nova服务发布

``tcp-demo/resource/config/${env}/nova.php``

.. code:: php

    <?php

    return [
        // 发布服务
        "novaApi" => [
            "path" => "vendor/nova-service/nova-demo/sdk/gen-php",
            // 注意namespace要与thrift声明一致
            "namespace" => "Com\\Yourcompany\\Demo\\",
        ],
    ];

http(nova客户端)项目 配置直接tcp-server，通过 nova 存根代码调用nova服务
-----------------------------------------------------------------------

配置拉取服务

``http-demo/resource/config/${env}/service_discovery.php``

.. code:: php

    <?php

    return [
        # 服务Provider应用名
        "app_names" => [
            "tcp-demo"
        ],

        "novaApi" => [
            // 这里配置与client相同
            "tcp-demo" => [
                "path" => "vendor/nova-service/nova-demo/sdk/gen-php",
                "namespace" => "Com\\Yourcompany\\Demo\\",
            ]
        ],

        "connection" => [
            // 直连地址
            // 端口参见 tcp-demo/resource/config/online/server.php host & port 配置
            "tcp-demo" => [
                "host" => "127.0.0.1",
                "port" => 8100,
            ],
        ],
    ];

调用代码：

.. code:: php

    <?php

    namespace Com\Youzan\ZanHttpDemo\Demo\Service;


    use Com\Yourcompany\Demo\Service\DemoService;

    class NovaCall
    {

        public function invokeRemoteNovaMethod()
        {
            $result = [];

            $service = new DemoService();
            $result['Bool'] = (yield $service->returnBool());
            $result['Void'] = (yield $service->returnVoid());
            $result['I32'] = (yield $service->returnI32());
            $result['Double'] = (yield $service->returnDouble());
            $result['String'] = (yield $service->returnString());
            $result['Enum'] = (yield $service->returnEnum());
            $result['BaseStruct'] = (yield $service->returnBaseStruct());

            $result['MixedStruct'] = (yield $service->returnMixedStruct());
            $result['List'] = (yield $service->returnList());
            $result['Set'] = (yield $service->returnSet());
            $result['Map'] = (yield $service->returnMap());

            yield $result;
        }
    }

分别启动tcp-demo、http-demo
---------------------------

tcp-demo

::

    [2017-06-05 12:31:02 #] Running in online mode
    [2017-06-05 20:31:02 #] server starting ..... [0.0.0.0:8100]
    [2017-06-05 20:31:02 #23827.0]  WARNING swReactorThread_onPipeReceive: [Master] set worker idle.[work_id=0]
    [2017-06-05 20:31:02 #23827.1]  WARNING swReactorThread_onPipeReceive: [Master] set worker idle.[work_id=1]
    [2017-06-05 20:31:02 #0] worker *0 starting .....
    [2017-06-05 20:31:02 #1] worker *1 starting .....
    [2017-06-05 20:31:02 #0] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:02 #1] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:02 #0] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:02 #1] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:02 #0] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:02 #0] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:02 #1] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:02 #1] mysql client connect to server [host=127.0.0.1, port=3306]

http-demo

::

    [2017-06-05 12:31:50 #] Running in online mode
    [2017-06-05 20:31:50 #23838.1]  WARNING swReactorThread_onPipeReceive: [Master] set worker idle.[work_id=1]
    [2017-06-05 20:31:50 #23838.0]  WARNING swReactorThread_onPipeReceive: [Master] set worker idle.[work_id=0]
    [2017-06-05 20:31:50 #] server starting .....[0.0.0.0:8030]
    [2017-06-05 20:31:51 #0] worker *0 starting .....
    [2017-06-05 20:31:51 #1] worker *1 starting .....
    [2017-06-05 20:31:51 #0] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:51 #1] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:51 #0] redis client connect to server [host=127.0.0.1, port=6379]
    [2017-06-05 20:31:51 #1] redis client connect to server [host=127.0.0.1, port=6379]

    # 这里观察日志，已经连接到tcp-demo

    [2017-06-05 20:31:51 #0] nova client connect to server [app_name=tcp-demo, host=127.0.0.1, port=8100, namespace=com.youzan.service, protocol=nova, status=1, weight=100]
    [2017-06-05 20:31:51 #1] nova client connect to server [app_name=tcp-demo, host=127.0.0.1, port=8100, namespace=com.youzan.service, protocol=nova, status=1, weight=100]


    [2017-06-05 20:31:51 #1] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:51 #0] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:51 #1] mysql client connect to server [host=127.0.0.1, port=3306]
    [2017-06-05 20:31:51 #0] mysql client connect to server [host=127.0.0.1, port=3306]

测试

curl (http)-> **http-demo** (nova)-> **tcp-demo**

curl http://127.0.0.1:8030/index/index/novaRemoteService

::

    {"code":0,"msg":"json string","data":{"Bool":true,"Void":null,"I32":768,"Double":0.17000000000000001,"String":"aaaa","Enum":2,"BaseStruct":{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null},"MixedStruct":{"propString":"MixedStruct","baseStruct":{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null},"propList":[{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}],"propSet":[{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}],"propMap":{"returnMixedStruct":{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}}},"List":[{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}],"Set":[{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}],"Map":{"returnMap":{"propBool":true,"propByte":0,"propI16":10,"propI32":100,"propI64":1000,"propDouble":1000,"propString":"BaseStruct","errorLevel":null}}}}⏎
