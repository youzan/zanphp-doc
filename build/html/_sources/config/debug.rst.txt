debug.php
=========

文件位置
~~~~~~~~

::

    resource/config/$ENV/debug.php

配置文件内容
~~~~~~~~~~~~

.. code:: php

    <?php

    return [
        //是否开启debug, 推荐test环境开启，其他环境关闭
        'debug' => true,
    ];
