Qrcode
======

zan框架屏蔽了二维码生成的细节，业务可以便捷使用接口生成二维码。

配置
~~~~

配置文件路径：resource/config/$ENV/services/qrcode.php。

.. code:: php

    <?php

    return [
        //二维码生成的server端host和port
        'host' => 'www.example.com',
        'port' => 8888,
    ];


接口
~~~~

.. code:: php

    class Qrcode {
        public static function create($data,$size='200x200',$base64=false, $styles = []);
    }

入口参数的含义为

-  txt: 二维码内容
-  size: 二维码大小
-  base64: 是否进行base64编码
-  styles：其他参数设定.

返回值：

生成的二维码字符串数据，前端可以直接使用。

使用示例
~~~~~~~~

.. code:: php

    namespace Com\Youzan\ZanHttpDemo\Controller\Index;
    use Zan\Framework\Foundation\Domain\HttpController as Controller;

    class IndexController extends Controller {
        $text = "youzan";
        $size = '270x270';
        $qrCode = (yield Qrcode::create($text, $size, true));
        if($isbase) {
            $response =  $this->output("<img src='{$qrcode}' />");
        }else{
            $response =  $this->output($qrCode);
            $response->withHeaders(['Content-Type' => 'image/jpg']);
        }
        yield $response;
    }
