hawk.php
========

File Directory
~~~~~~~~~~~~~~

::

    resource/config/$ENV/hawk.php

Config Effect
~~~~~~~~~~~~~

config about post hawk monitor data

Config File Content
~~~~~~~~~~~~~~~~~~~

.. code:: php

    <?php

    return [
        //是否运行, pre不需要运行
        'run' => false,
        'host' => '1.1.1.1',
        'port' => 1234,
        'uri' => '/monitor/push',
        //多久上报一次，单位毫秒
        'time' => 60000,
    ];
