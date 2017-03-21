# 协议

zan框架Tcp服务主要采用内部定制的thrift协议-Nova，主要针对现有的thrift进行了一层封装改造，协议的格式为：

#### **请求/响应协议**

| Header | message size | **4 bytes** |
| :---: | :---: | :---: |
|  | magic | 2 bytes |
|  | header size | 2 bytes |
|  | version | 1 bytes |
|  | IP | 4 bytes |
|  | port | 4 bytes |
|  | service name size | 4 bytes |
|  | service name |  |
|  | method name size | 4 bytes |
|  | method name |  |
|  | request id/response id | 8 bytes |
|  | attachment size | 4 bytes |
|  | attachment |  |
| Body | message body | thrift serialize |

各字段的含义为：

* message size:整个协议的长度
* magic:魔数，用于识别协议，如果无法识别，将消息做丢弃处理----0xDABC
* header size:包头长度
* version:协议版本
* IP:调用端IP
* port:调用端socket出口port
* service name:服务名
* method name:方法名
* request id=response id:消息标识
* attachment size:附加消息大小
* attachment:附加消息，json
* message body:使用thrift序列化部分



