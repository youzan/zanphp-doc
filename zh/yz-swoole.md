# 安装yz-swoole


> 目前yz-swoole仅在内部使用，使用开源版本请使用swoole-1.8.5



1. ````yum install -y yz-swoole````
2. ````$YOUR_PHP_BIN_DIR/phpize````
3. ````./configure --with-php-config=$YOUR_PHP_BIN_DIR/php-config --enable-async-mysql --enable-sockets --enable-async-redis --enable-aerospike````
4. ````make install````
5. 在php.ini或者php.d/swoole.ini中加入 ````extension=$SWOOLE_SO_PATH/swoole.so````
6. ````php -m | grep swoole```` 看到有swoole即可。

