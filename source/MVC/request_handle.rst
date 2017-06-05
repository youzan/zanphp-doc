请求处理
========

Request
-------

zan框架提供操作request的类和方法，文件位于src/Network/Http/Request/Request.php。类中方法包括

.. code:: php

    //访问的url为http://127.0.0.1:8030/index/index/getUrl?a=1&b=2
    //下列接口对应的返回值
    //http://127.0.0.1:8030/index/index/getUrl
    public function getUrl()
    //http://127.0.0.1:8030/index/index/getUrl?a=1&b=2
    public function getFullUrl()
    //index/index/getUrl
    public function getPath()
    //index/index/getUrl
    public function getDecodedPath()
    /*array(3) {
      [0] =>
      string(5) "index"
      [1] =>
      string(5) "index"
      [2] =>
      string(6) "getUrl"
    }*/
    public function getSegments()

    //下列方法对应php中的全局变量_GET、_POST、_COOKIE、_SERVER数组，$key为null时直接返回数组，否则返回对应key的值
    public function get($key = null, $default = null)
    public function post($key = null, $default = null)
    public function cookie($key = null, $default = null)
    //请求相关的server信息，如调用者ip和port等信息
    public function server($key = null, $default = null)
    //HTTP包中的头信息数组
    public function header($key = null, $default = null)
    //HTTP POST的json字符串
    public function json($key = null, $default = null)

Context
-------

context包含了请求的上下文信息，如request、session等，使用方式为

.. code:: php

     $request = (yield getContext('request'));
     $request = (yield getContext('session'));

Session
-------

从context获取session之后，session的操作方法包括

.. code:: php

    class Session {
        //session中设置key-value
        public function set($key, $value);
        //获取session中key的值
        public function get($key);
        //删除session，使之失效
        public function destory();
        //删除session中某个key的值
        public function delete($key);
    }

Response
--------

zan框架的Controller中返回的都是Zan\Framework\Network\Http\Response\Response对象，对象中方法包括

.. code:: php

    public function withHeader($key, $value, $replace = true)
    public function withHeaders(array $headers)
    public function withCookie($cookie)
    public function withCookies(array $cookies)
    public function setContent($content)

withHeader和withHeaders设置响应中的头部信息，withCookie和withCookies设置响应中的cookie信息，setContent设置响应包内容。

辅助方法
--------

获取客户端ip
^^^^^^^^^^^^

.. code:: php

    $request->getClientIp()

在通过反向代理（如nginx）访问server时，此方法需要在resource/config/$ENV/server.php中配置trust
proxy ip，配置示例见\ `server.php <../config/server.html>`__\ 中的proxy字段。
