.. _eth-net:

=============
web3.eth.net
=============


包含获取当前网络信息的一些函数。

------------------------------------------------------------------------------


.. include:: include_package-net.rst


------------------------------------------------------------------------------

getNetworkType
=====================

.. code-block:: javascript

    web3.eth.net.getNetworkType([callback])

通过比较创世区块哈希值来猜测当前节点连的是哪条链。

.. note:: 推荐使用 :ref:`web3.eth.getChainId <eth-chainId>` 来检测现在所连的链。

-------
返回值
-------

``Promise<String>``:
    - ``"main"`` 代表主网
    - ``"morden"`` 代表 morden 测试网
    - ``"ropsten"`` 代表 ropsten 测试网
    - ``"private"`` 表示检测不到当前网络


-------
例子
-------

.. code-block:: javascript

    web3.eth.net.getNetworkType()
    .then(console.log);
    > "main"


