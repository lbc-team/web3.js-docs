
.. _net-getid:

getId
=========

.. code-block:: javascript

    web3.eth.net.getId([callback])
    web3.bzz.net.getId([callback])
    web3.shh.net.getId([callback])

获取当前的网络 ID.

----------
参数
----------

none

-------
返回值
-------

``Promise`` 返回 ``Number``: 网络 ID.

-------
例子
-------

.. code-block:: javascript

    web3.eth.net.getId()
    .then(console.log);
    > 1

------------------------------------------------------------------------------


isListening
=========

.. code-block:: javascript

    web3.eth.net.isListening([callback])
    web3.bzz.net.isListening([callback])
    web3.shh.net.isListening([callback])

查看当前节点是否正在连接其它对等节点。

----------
参数
----------

none

-------
返回值
-------

``Promise`` 返回 ``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.eth.isListening()
    .then(console.log);
    > true

------------------------------------------------------------------------------

getPeerCount
=========

.. code-block:: javascript

    web3.eth.net.getPeerCount([callback])
    web3.bzz.net.getPeerCount([callback])
    web3.shh.net.getPeerCount([callback])

获取正在连接的对等节点的数量。

----------
参数
----------

none

-------
返回值
-------

``Promise`` 返回 ``Number``

-------
例子
-------

.. code-block:: javascript

    web3.eth.getPeerCount()
    .then(console.log);
    > 25
