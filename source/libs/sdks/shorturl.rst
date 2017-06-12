ShortUrl
========

zan框架封装了短地址接入的api，业务开发可以直接通过long
url查询对应的short url。

配置
~~~~

Shorturl配置在项目resource/config/$ENV/shorturl.php下。

.. code:: php

    return  [
        //短地址接入的server地址
        'host' => 'www.example.com',
        'port' => 80
    ];

接口
~~~~

.. code:: php

    class ShortUrl {
        public static function get($url);
    }

get方法入参为long url，返回的是short url。

使用示例
~~~~~~~~

.. code:: php

    $shortUrl = (yield ShortUrl::get("http://www.example.com"));
