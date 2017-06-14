====
简介
====

.. image:: https://github.com/youzan/zanphp.io/blob/master/src/img/zan-logo-small@2x.png?raw=true
  :height: 210 px
  :width: 210 px
  :alt: zanphp logo
  :align: center

ZanPHP是基于PHP协程的网络服务框架，提供最简单的方式开发面向C10K+的高并发SOA服务和RPC服务。
每天为2,000+个服务提供300,000,000+次访问量支持，广泛应用于有赞各项业务。

核心特性
========

1.  基于 ``yield`` 实现了独立堆栈的协程
2.  类似于 Golang 的并发编程模型实现
3.  基于 zan 提供异步非阻塞I/O服务
4.  连接池支持（内置MySQL、Redis、syslog等多种组件）
5.  类似 Golang 的defer机制解决由于异常导致的资源未释放、锁未释放的问题
6.  可继承的View布局及组件化支持，方便完成bigPipe/bigRender/首屏加载优化等不同的渲染方式
7.  基于模型驱动的SQLMap，实现了SQL的快速定位及方便的sharding、cache支持
8.  提供类似于 `Laravel <https://github.com/laravel/laravel>`__
    的middleware(Filters & Terminators)机制
9.  Di及单元测试的良好支持
10. 完整的RPC远程服务调用方案

框架定位
========

ZanPHP 的定位是高并发 Web 服务或业务中间件。

ZanPHP 参考了很多 Golang 特性，不过目的绝不是为了替换掉 Golang。

PHP 在业务系统开发上的优势明显，而 Golang 相信会是将来系统编程的霸主。

ZanPHP 和 Golang 的边界是：ZanPHP做业务系统；Golang
做平台系统（中间件或基础服务组件）。

而 ZanPHP 和 Golang 编程模型的驱近，是希望能给PHP程序员一个更好的桥梁到Golang。

理想的技术栈是：ZanPHP + Go + 少量的C/C++。

当然对于致力于终身coding的码农来说：Java依然很难跨过去的坎。

官方文档
========

ZanPHP的文档仓库地址：\ `zanphp-doc <https://github.com/youzan/zanphp-doc>`__\ 。

常用链接
========

-  `zan-doc <https://github.com/youzan/zanphp-doc>`__ - Zan PHP 开发者文档
-  `zan-installer <https://github.com/youzan/zan-installer>`__ - Zan PHP
   脚手架工具
-  `zanhttpdemo <https://github.com/youzan/zanhttpdemo>`__ - Zan PHP HTTP demo
-  `zantcpdemo <https://github.com/youzan/zantcpdemo>`__ - Zan PHP TCP demo

开发交流
========

QQ群：115728122

License
=======

我们\ `有赞 <https://youzan.com/>`__\ 的 `Zan
PHP框架 <https://github.com/youzan/zanphp>`__\ 基于 `MIT
license <https://opensource.org/licenses/MIT>`__ 进行开源。
