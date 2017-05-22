UUID
====

zan框架封装了redis uuid生成器的调用接口，业务可以直接调用。

配置文件
~~~~~~~~

配置文件即redis配置路径：resource/config/$ENV/redis.php，增加uuid的connection即可。

.. code:: php

    <?php

    return [
        'uuid' => [
            'engine'=> 'redis',
            //uuid生成器server地址
            'host' => 'idgen-qa.s.qima-inc.com',
            'port' => 6000,
            'pool'  => [
                'maximum-connection-count' => '50',
                'minimum-connection-count' => '10',
                'keeping-sleep-time' => '10',
                'init-connection'=> '2',
            ],
        ],
    ];

接口
~~~~

.. code:: php

    class RedisUuid {
        public function get($tableName);
        public function getSerialId();
        public function getSnowflake();
        public function getObjectId();
    }

提供的uuid生成器方法分别为：

-  自增整数序列(64位整数):获取的id是一递增的序列,灵感来自与mysql的自增id,每秒生成的id数取决于redis-server本身的性能,
   目前的物理机,单机单进程可达13w，瓶颈在于io，因为是通过redis的aof
   always来做数据的持久化
-  serialid(64位整数,人类可读):实现的id算法,
   4位无用位+40位秒级时间戳+2位数据中心id+2位进程id+15位自增序列,每秒可以生成2^15
   (32768)个id
-  snowflake(64位整数)：由twitter提出的snowflake算法,1位无用位+41位毫秒级时间戳+10位机器id+12位自增序列,
   每毫秒可以生成2^12(4096)个id
-  objectid(24字节字符串):mongodb实现的id算法,
   4字节的时间戳+3字节的数据中心id+2字节的进程id+3字节的自增序列,
   每秒可以生成2^24(16777216)个id

使用示例
~~~~~~~~

.. code:: php

    $tableName = 'unit_test_uuid_table';
    $id = (yield RedisUuid::getInstance()->get($tableName));
    $serialId = (yield RedisUuid::getInstance()->getSerialId());
    $snowflake = (yield RedisUuid::getInstance()->getSnowflake());
    $objId = (yield RedisUuid::getInstance()->getObjectId());
