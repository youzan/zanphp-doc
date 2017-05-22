log.php
=======

文件位置
~~~~~~~~

::

    resource/config/$ENV/log.php

配置作用
~~~~~~~~

日志输出方式和目的地址配置

配置文件内容
~~~~~~~~~~~~

.. code:: php

    return [
        // 日志查看 http://log.qima-inc.com
        'default' => 'syslog://info/syslog_default?module=default',
        //zan框架内置配置，无需设置，直接使用
        'zan_framework' => 'syslog://info/zan_framework?module=soa-framework', 
    ];

以上面的debug配置为例，日志目的地址由一条url表示：

-  scheme代表日志类型，目前支持file（绝对路径）、log（项目相对路径，会写到项目的resource/log目录下）、syslog（flume
   log系统，内部系统，暂无开源计划，如需使用需自行开发）、blackhole（黑洞）;
-  host
   表示日志级别，由低到高为debug、info、notice、warning、error、critical、alert和emergency;
-  path表示\ `log连接池 <../libs/pool/log.md>`__\ 中的key（最终实际路径为项目的resource/log/debug.log）;
-  query为其他选项设置，如useBuffer=true&format=json 再次解析之后表示
   “启用buffer” 和
   “格式化为json的形式存入log文件”，module=soa-framework设置日志输出信息中的模块名为soa-framework。
