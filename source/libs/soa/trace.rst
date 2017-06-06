Trace
=====

本篇文档业务开发无需关心，只针对于框架开发。

为展示服务间的调用关系，明确整个请求生命周期中所经历的各个阶段，统计各请求的成功率和耗时情况，完成请求的全链路监控，zan框架实现了调用链机制。

1.初始化
~~~~~~~~

调用链会在filter中初始化，如果发现有parentId、rootId、eventId则不会生成新的rootId和parentId，这些id目前只支持通过nova协议传输，也就是说只有tcp
server才能做非第一层的调用链传递。

2.打点
~~~~~~

目前调用链已经在novaclient, db,
httpclient进行打点，后续还会继续扩大打点范围。

3.上报
~~~~~~

调用链会在TraceTerminator中进行上报数据，上报目的地址配置位于tcp连接池\ `connection/tcp.php <../pool/tcp.html>`__。
