使用示例
--------

文件目录
~~~~~~~~

.. code:: php

    ├── Controller
    │   ├── Index
    │   │   └── IndexController.php
    │   └── Test
    │       └── ExceptionController.php
    ├── Demo
    │   ├── Service
    │   │   └── HttpCall.php
    │   └── View
    │       └── test
    │           └── test.html
    └── Model
        └── Index
            └── GetAllDemoData.php

IndexController.php
~~~~~~~~~~~~~~~~~~~

.. code:: php

    <?php

    namespace Com\Youzan\ZanHttpDemo\Controller\Index;

    use Com\Youzan\ZanHttpDemo\Demo\Service\HttpCall;
    use Zan\Framework\Foundation\Domain\HttpController as Controller;
    use Com\Youzan\ZanHttpDemo\Model\Index\GetAllDemoData;

    class IndexController extends Controller {

        //字符串输出示例
        public function index()
        {
            $response =  $this->output('success');
            //设置响应信息头部
            $response->withHeaders(['Content-Type' => 'text/javascript']);
            yield $response;
        }

        //json输出示例
        public function json()
        {
            yield $this->r(0, 'json string', ["Hello" => "World"]);
        }

        //模板输出示例
        public function showTpl()
        {
            //给模板中的变量赋值
            $this->assign("str", "Zan Framework");
            //输出模板页面
            yield $this->display("Demo/test/test");
        }

        //操作数据库示例
        public function dbOperation()
        {
            $demo = new GetAllDemoData();
            //执行sql语句
            yield $demo->doSql();
        }

        //Http服务调用示例
        public function rpcRemoteService()
        {
            $http = new HttpCall();
            yield $http->visit();
        }
    }

ExceptionController
~~~~~~~~~~~~~~~~~~~

.. code:: php

    <?php

    namespace Com\Youzan\ZanHttpDemo\Controller\Test;

    use Zan\Framework\Foundation\Domain\HttpController as Controller;
    use Zan\Framework\Network\Http\Exception\InvalidRouteException;
    use Zan\Framework\Network\Http\Exception\PageNotFoundException;
    use Zan\Framework\Network\Http\Exception\RedirectException;

    class ExceptionController extends Controller
    {
        public function testInvalidRoute()
        {
            throw new InvalidRouteException();
        }

        public function testPageNotFound()
        {
            throw new PageNotFoundException('This Page is not Found');
        }

        public function testRedirect()
        {
            throw new RedirectException('https://youzan.com', 'Test Redirect');
        }
    }

HttpCall.php
~~~~~~~~~~~~

.. code:: php

    <?php

    namespace Com\Youzan\ZanHttpDemo\Demo\Service;

    use Zan\Framework\Network\Common\Client;

    class HttpCall {

        public function visit()
        {
            $option = [
                'order_no'     => 1,
                'kdt_id'       => 1,
                'format_order' => false,
                'with_items'   => false,
                'with_peerpay' => false,
                'with_source'  => false
            ];

            //trade.order.detail.byOrderNo对应包zan-config/zan/Apiconfig.php中的配置,新增url时需要修改配置
            yield Client::call('trade.order.detail.byOrderNo', $option);
        }
    }

Test.html
~~~~~~~~~

.. code:: php

    <?php
        echo "Hello World $str"; //$str在调用时使用assgin方法赋值
    ?>

GetAllDemoData.php
~~~~~~~~~~~~~~~~~~

.. code:: php

    <?php

    namespace Com\Youzan\ZanHttpDemo\Model\Index;

    use Zan\Framework\Store\Facade\Db;

    class GetAllDemoData {
        public function doSql()
        {
            $data = [
                'limit' => 2
            ];
            //demo.demo_sql_id1_1对应resource/sql/demo.php中的配置
            $result = (yield Db::execute("demo.demo_sql_id1_1", $data));
            var_dump($result);
        }
    }
