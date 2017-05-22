原生Sql
=======

除了SqlMap，zan框架还提供了原生sql执行的接口，开发人员在自行保证sql语句的安全性的前提下，使用起来十分便捷有效。

.. code:: php

    class Flow {
        public function queryRaw($table, $sql);
    }

table：sql作用的数据库表名，需要在resource/config/share/table/
目录下建立数据表与连接池配置的映射关系

sql：待执行的原生sql语句

示例
----

.. code:: php

    $flow = new Flow();
    $table = "market_category";
    $sql = "SELECT * FROM market_category LIMIT 2";
    yield $flow->queryRaw($table, $sql);
