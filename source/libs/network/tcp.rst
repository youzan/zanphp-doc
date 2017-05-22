Tcp
===

Tcp
Client的使用示例如下，创建TcpClient实例之后yield调用send方法异步发送数据，接收响应。返回结果为字符串数据。

.. code:: php

    <?php
    $connection = (yield ConnectionManager::getInstance()->get("tcp.trace"));
    $tcpClient = new TcpClient($connection);
    $data = "Hello world";
    $result = (yield $tcpClient->send($data));
