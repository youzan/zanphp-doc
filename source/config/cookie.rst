cookie.php
=========

文件位置
~~~~~~~~

::

    resource/config/$ENV/cookie.php

配置作用
~~~~~~~~

HTTP server的cookie使用配置

配置文件内容
~~~~~~~~~~~~

.. code:: php

	<?php
    //参数用法见PHP官方cookie文档
	return [
		'domain' => '.xxx.com',
		'path' => '/',
		'expire' => 2592000,
		'secure' => FALSE,
		'httponly' => FALSE,
	];

