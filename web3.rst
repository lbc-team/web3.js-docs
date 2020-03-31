
====
Web3
====

Web3 是 web3.js 库的主类。

.. code-block:: javascript

    var Web3 = require('web3');

    > Web3.utils
    > Web3.version
    > Web3.givenProvider
    > Web3.providers
    > Web3.modules

------------------------------------------------------------------------------

Web3.modules
=====================

.. code-block:: javascript

    Web3.modules

将返回所有主要子模块类的对象，以便能够手动实例化它们。

-------
返回值
-------

``Object``: 模块构造函数列表:
    - ``Eth`` - ``Constructor``: 与以太坊网络交互的 Eth 模块，查看 :ref:`web3.eth <eth>` 了解更多。
    - ``Net`` - ``Constructor``: 与网络属性进行交互的 Net 模块，查看 :ref:`web3.eth.net <eth-net>` 了解更多。
    - ``Personal`` - ``Constructor``: 与以太坊账户交互的 Personal 模块，查看  :ref:`web3.eth.personal <personal>` 了解更多。
    - ``Shh`` - ``Constructor``: 与whisper协议交互的 Shh 模块，查看 :ref:`web3.shh <shh>` 了解更多。
    - ``Bzz`` - ``Constructor``: 与swarm网络交互的 Bzz 模块，查看 :ref:`web3.bzz <bzz>` 了解更多。

-------
例子
-------

.. code-block:: javascript

    Web3.modules
    > {
        Eth: Eth(provider),
        Net: Net(provider),
        Personal: Personal(provider),
        Shh: Shh(provider),
        Bzz: Bzz(provider),
    }


------------------------------------------------------------------------------

Web3 实例
=============

Web3类是一个“伞”包，在Web3类下包含所有与以太坊相关的模块。


.. code-block:: javascript

    var Web3 = require('web3');

    // 创建实例，如果在支持以太坊的浏览器 "Web3.providers.givenProvider" 会被设置。
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    > web3.eth
    > web3.shh
    > web3.bzz
    > web3.utils
    > web3.version


------------------------------------------------------------------------------

version
============

    ``version`` 即是Web3类的静态可访问属性也Web3实例的属性。

.. code-block:: javascript

    Web3.version
    web3.version

返回当前 web3.js 库的软件包版本。

-------
返回值
-------

``String``: 当前版本。

-------
例子
-------

.. code-block:: javascript

    web3.version;
    > "1.2.3"



------------------------------------------------------------------------------


utils
=====================

    ``utils`` 即是Web3类的静态可访问属性也Web3实例的属性。

.. code-block:: javascript

    Web3.utils
    web3.utils

web3.utils的工具方法也直接在Web3类对象上公开（译者注：即可以直接通过 Web3 来访问工具方法）。


查看 :ref:`web3.utils <utils>` 了解更多。


------------------------------------------------------------------------------


.. include:: include_package-core.rst


------------------------------------------------------------------------------
