安装
==================

Zan PHP 依赖以下包

`zan <https://github.com/youzan/zan>`__

https://github.com/youzan/zan.git

`php-lz4扩展 <https://github.com/kjdev/php-ext-lz4>`__

https://github.com/kjdev/php-ext-lz4.git

`php-apcu扩展 <https://github.com/krakjoe/apcu>`__

https://pecl.php.net/package/APCu

注：zan扩展与swoole扩展不兼容，且opcache扩展在php7下不稳定，建议使用zan框架时关闭swoole扩展和opcache扩展配置。

扩展安装完成之后，利用脚手架`zan—installer <http://zanphp.io/guide/cli>`__安装zanhttpdemo和zantcpdemo即可。