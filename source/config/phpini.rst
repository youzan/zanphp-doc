php.ini
=======

执行命令获取php.ini文件位置

::

    php -i|grep php.ini

在php.ini文件末尾添加

.. code:: php

    date.timezone = "Asia/Chongqing"
    zanphp.RUN_MODE = test
    zanphp.DEBUG = true

-  date.timezone：设置时间时区

-  zanphp.RUN\_MODE：程序运行模式，对应resource/config下的子目录，如zanphp.RUN\_MODE
   = test对应配置resource/config/test生效，默认值为online,可选的环境配置包括dev,test,
   pre,readonly,online,unittest,qatest,pubtest,ci,perf,daily。
-  zanphp.DEBUG：是否开启调试模式，默认值为false
