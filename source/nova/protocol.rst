NOVA协议
========

zan框架Tcp服务主要采用内部定制的thrift协议-Nova，主要针对现有的thrift进行了一层封装改造，协议的格式为：

**请求/响应协议**
^^^^^^^^^^^^^^^^^

+----------+--------------------------+--------------------+
| Header   | message size             | **4 bytes**        |
+==========+==========================+====================+
|          | magic                    | 2 bytes            |
+----------+--------------------------+--------------------+
|          | header size              | 2 bytes            |
+----------+--------------------------+--------------------+
|          | version                  | 1 bytes            |
+----------+--------------------------+--------------------+
|          | IP                       | 4 bytes            |
+----------+--------------------------+--------------------+
|          | port                     | 4 bytes            |
+----------+--------------------------+--------------------+
|          | service name size        | 4 bytes            |
+----------+--------------------------+--------------------+
|          | service name             |                    |
+----------+--------------------------+--------------------+
|          | method name size         | 4 bytes            |
+----------+--------------------------+--------------------+
|          | method name              |                    |
+----------+--------------------------+--------------------+
|          | request id/response id   | 8 bytes            |
+----------+--------------------------+--------------------+
|          | attachment size          | 4 bytes            |
+----------+--------------------------+--------------------+
|          | attachment               |                    |
+----------+--------------------------+--------------------+
| Body     | message body             | thrift serialize   |
+----------+--------------------------+--------------------+

各字段的含义为：

-  message size:整个协议的长度
-  magic:魔数，用于识别协议，如果无法识别，将消息做丢弃处理----0xDABC
-  header size:包头长度
-  version:协议版本
-  IP:调用端IP
-  port:调用端socket出口port
-  service name:服务名
-  method name:方法名
-  request id=response id:消息标识
-  attachment size:附加消息大小
-  attachment:附加消息，json
-  message body:使用thrift序列化部分


两点说明：

- 包头service:method与messagethrift序列化部分重复原因，fpm短连接模型下nova服务调用经由local-proxy，proxy只解析包头进行转发
- ip:port与tcp连接属性重复，也是因为proxy存在, 透传调用方的ip+port给nova-server

附录:

.. code:: c
	
	typedef struct swNova_Header {
    	int32_t     msg_size;
    	uint16_t    magic;
    	int16_t     head_size;
    	int8_t      version;
    	uint32_t    ip;
    	uint32_t    port;
    	int32_t     service_len;
    	char        *service_name;
    	int32_t     method_len;
    	char        *method_name;
    	int64_t     seq_no;
    	int32_t     attach_len;
    	char        *attach;
	} swNova_Header;

