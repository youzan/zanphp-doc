Monitor
=======

zan框架内置了统一的监控上报接口（hawk），上报信息至ZCloud应用管理平台。

配置
~~~~

监控的配置参见\ `hawk.php <../config/hawk.md>`__

接口
~~~~

.. code:: php

    class Hawk {
        //请求成功耗时
        public function addTotalSuccessTime($side, $service, $method, $ip, $diffSec);
        //请求失败耗时
        public function addTotalFailureTime($side, $service, $method, $ip, $diffSec);
        //增加成功请求个数
        public function addTotalSuccessCount($side, $service, $method, $ip);
        //增加失败请求个数
        public function addTotalFailureCount($side, $service, $method, $ip);
        /**
         * array(
         *  'biz' => 'worker_memory',
         *  'metrics' => [
         *      'used' => 234234234,
         *  ],
         *  'tags' => [
         *      'application' => 'pf-web',
         *      'work_id' => '2',
         *      'host' => 'bc_sdfs',
         *  ],
         * ),
         * @param $biz
         * @param array $metrics
         * @param array $tags
         */
        public function add($biz, array $metrics, array $tags = [])
    }

入参的含义分别为

-  side：标识server还是client上报，取值为Hawk::SERVER或Hawk::CLIENT
-  service：服务名称
-  method：方法名称
-  ip：请求源ip地址
-  diffSec：耗时长度，单位为秒
-  biz：业务属性
-  metric：业务数据
-  tags：存放额外信息的标签

使用示例
~~~~~~~~

.. code:: php

    $hawk = Hawk::getInstance();
    $hawk->addTotalSuccessTime(Hawk::SERVER,
        $request->getServiceName(),
        $request->getMethodName(),
        $request->getRemoteIp(),
        microtime(true) - $request->getStartTime()
    );
    $hawk->addTotalSuccessCount(Hawk::SERVER,
        $request->getServiceName(),
        $request->getMethodName(),
        $request->getRemoteIp()
    );

    $hawk->add('youzan.soa', $metrics, $tags);
