# 请求处理

## Request

zan框架提供操作request的类和方法，文件位于src/Network/Http/Request/Request.php。类中方法包括

```php
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
public function server($key = null, $default = null)
//HTTP包中的头信息数组
public function header($key = null, $default = null)
//HTTP POST的json字符串
public function json($key = null, $default = null)
```

## Context

context包含了请求的上下文信息，如request、session、cookie等，使用方式为

```php
 $request = (yield getContext('request'));
 $request = (yield getContext('session'));
 $request = (yield getContext('cookie'));
```

## Session

```

```

## Cookie

```

```

## Response

zan框架的Controller中返回的都是Zan\Framework\Network\Http\Response\Response对象，对象中方法包括

```php
public function withHeader($key, $value, $replace = true)
public function withHeaders(array $headers)
public function withCookie($cookie)
public function withCookies(array $cookies)
public function setContent($content)
```

withHeader和withHeaders设置响应中的头部信息，withCookie和withCookies设置响应中的cookie信息，setContent设置响应包内容。

