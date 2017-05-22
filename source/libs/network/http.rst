Http
====

Http Client目前支持两种使用方式：

-  原生HttpClient
-  Zan\Framework\Network\Common\Client::call

原生HttpClient
~~~~~~~~~~~~~~

HttpClient的使用示例如下，创建HttpClient实例之后，yield调用get方法即可异步返回结果。返回结果为Zan\Framework\Network\Common\Response对象，可以通过getBody、getHeaders和getStatusCode获取响应的包内容、头部和状态码。

HttpClient的构造方式主要有以下三种：

.. code:: php

    1.$httpClient = new HttpClient($host, $port);
    2.$httpClient = HttpClient::newInstance($host, $port = 80, $ssl = false);
    3.$httpClient = HttpClient::newInstanceUsingProxy($host, $port = 80, $ssl = false);

参数ssl表示是否为https连接，访问https的url时，传入ssl为true。

第一种方式与第二种等价，第三种通过proxy建立http连接，proxy各环境的地址为（端口号80）见

http://doc.qima-inc.com/pages/viewpage.action?pageId=4851473

http client目前支持get、post和postJson方法。

.. code:: php

    $httpClient = new HttpClient($host, $port);
    $params = [];
    $timeout = 60;
    $response = (yield $httpClient->get($uri, $params, $timeout));
    $result = $response->getBody();

Client::call
~~~~~~~~~~~~

client::call对HttpClient进行了封装，通过将所有访问目的地址信息聚集到统一配置文件中，业务方使用时就只需要指定配置目录的层级关系即可代替url，接口API为：

.. code:: php

    client::call($api, $params = [],$callback = null, $method = 'POST',$format='form')

api为目的url在ApiConfig.php中的层级位置，params为请求的参数，$callback保留，目前未使用。

$method取值'POST'或'GET'，表示请求的方法

$format定义POST的body格式，取值'format'或'json'，分别表示content-type为'application/x-www-form-urlencoded'和'application/json'
