# Trace

> 本篇文档业务开发无需关心，只针对于框架开发。

### 调用链初始化
调用链会在filter中初始化，如果发现有parentId、rootId、eventId则不会生成新的rootId和parentId，这些id目前只支持通过nova协议传输，也就是说只有tcp server才能做非第一层的调用链传递。

### 调用链的打点
目前调用链已经在novaclient, db, httpclient进行打点，后续还会继续扩大打点范围。

### 调用链的上报
调用链会在TraceTerminator中进行上报数据。