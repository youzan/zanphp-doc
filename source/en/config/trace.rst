trace.php
=========

File Directory
~~~~~~~~~~~~~~

::

    resource/config/$ENV/monitor/trace.php

Config Effect
~~~~~~~~~~~~~

config about call chain

Config File Content
~~~~~~~~~~~~~~~~~~~

.. code:: php

    <?php

    return [
        //是否开启调用链上报
        //如果开启需要先配置trace所需的tcp连接池
        "run" => false,
    ];
