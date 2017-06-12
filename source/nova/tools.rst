NOVA 配套工具
=============

nova 命令行工具
---------------

-  单文件轻量级nova命令行客户端 (php 5.6+ & swoole)；

-  内部泛化调用实现，手工编解码，不依赖具体nova-service Thrift stub包；

安装
~~~~

::

    php zanphp/src/toolkil/nova.php install

参数说明
~~~~~~~~

::

    Usage: nova -h主机 -p端口 -m方法 -a参数
        -h nova 服务主机
        -p nova 服务端口
        -m nova服务.nova方法 
            例如(以下方thrift为例)
                ${thrift-namespace}.${thrift-service}.${thrift-method}
                com.yourcompany.demo.service.demoService.returnString
        -a nova服务参数, json字符串
            例如 complexMethod 方法
            '{"paraBool":true, "paraI32": 42,"paraDouble":3.14, "paraString":"hello", "baseStruct":{}, "returnList":[{}], "returnSet":{}, "returnMap":{}, "errorLevel":1}'

使用
~~~~

服务 tcp-demo 项目服务：

.. code:: thrift

    namespace nova com.yourcompany.demo.service

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

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.returnVoid -a='{}'

    {
        "code": 0,
        "message": "Service \"Com\\Yourcompany\\Demo\\Service\" not found"
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.returnVoid -a '{}'

    null

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.returnString -a '{}'

    "aaaaaaaaaaaaaaaaaaaa"

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.returnBaseStruct -a '{}'

    {
        "propBool": true,
        "propByte": "a",
        "propI16": 10,
        "propI32": 100,
        "propI64": 1000,
        "propDouble": 1000,
        "propString": "BaseStruct",
        "errorLevel": null
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.paraBaseNoReturn -a '{}'

    {
        "code": 0,
        "message": "Too few arguments to paraBaseNoReturn, 0 passed and at least 2 expected"
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.complexMethod -a '{"paraBool":true}'

    {
        "code": 0,
        "message": "Too few arguments to complexMethod, 1 passed and at least 9 expected"
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.complexMethod -a '{"paraBool":true, "paraI32": 42,"paraDouble":3.14, "paraString":"hello", "baseStruct":null, "returnList":[1,2,3], "returnSet":null, "returnMap":null, "errorLevel":1}'

    {
        "code": 0,
        "message": "Invalid <baseStruct> type, expects object"
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.complexMethod -a '{"paraBool":true, "paraI32": 42,"paraDouble":3.14, "paraString":"hello", "baseStruct":{}, "returnList":[1,2,3], "returnSet":null, "returnMap":null, "errorLevel":1}'

    {
        "code": 0,
        "message": "Invalid <returnList[0]> type, expects object"
    }

::

    nova -h127.0.0.1 -p8100 -mcom.yourcompany.demo.service.demoService.complexMethod -a '{"paraBool":true, "paraI32": 42,"paraDouble":3.14, "paraString":"hello", "baseStruct":{}, "returnList":[{}], "returnSet":{}, "returnMap":{}, "errorLevel":1}'

    {
        "paraBool": true,
        "paraI32": 42,
        "paraDouble": 3.1400000000000001,
        "paraString": "hello",
        "baseStruct": {
            "propBool": null,
            "propByte": null,
            "propI16": null,
            "propI32": null,
            "propI64": null,
            "propDouble": null,
            "propString": null,
            "errorLevel": null
        },
        "returnList": [
            {
                "propBool": null,
                "propByte": null,
                "propI16": null,
                "propI32": null,
                "propI64": null,
                "propDouble": null,
                "propString": null,
                "errorLevel": null
            }
        ],
        "returnSet": [],
        "returnMap": [],
        "errorLevel": 1
    }
