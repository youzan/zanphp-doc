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
        'host' => 'api.kdt.im',
        'port' => 80
    ];

各环境下的server配置参见\ http://doc.qima-inc.com/pages/viewpage.action?pageId=10036541\ 。

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

    $shortUrl = (yield ShortUrl::get("http://koudaitong.com"));
