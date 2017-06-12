Load balance
============

提供同一个服务的多个nova
server对应连接池中的一个连接，在连接的选择上，zan框架采用加权轮询的负载均衡策略。

配置
~~~~

服务负载均衡的配置之中，权重越大的server调度优先级越高。

算法流程
~~~~~~~~

假设有一组服务器 S = {S0, S1, …, Sn-1}有相应的权重{W0, W1, ...,
Wn}，currentWeight为选取出的server需要的最低权重值，maxWeight为所有服务器中的最大权重值，gcdWeight是所有权重的最大公约数。

1. 每一轮循环回到S0后，currentWeight减小gcdWeight，如果currentWeight <
   0，重新赋值为maxWeight。
2. 从S0开始循环遍历所有服务器，找到第一个权重大于currentWeight的服务器返回。

对应的伪代码为：

.. code:: php

    while (true) {
        currentIndex = (currentIndex + 1) % serverCount;
        if (currentIndex == 0) {
            currentWeight = currentWeight - gcdWeight;
            if (currentWeight <= 0) {
                currentWeight = maxWeight;
            }
        }

        if (servers[currentIndex]['weight'] >= currentWeight)
            return servers[currentIndex];
    }
