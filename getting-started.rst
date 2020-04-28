
===============
Web3.js 入门
===============

web3.js 库是一系列模块的集合，服务于以太坊生态系统的各个功能，如：


- ``web3-eth`` 用来与以太坊区块链及合约的交互；
- ``web3-shh`` Whisper 协议相关，进行p2p通信和广播；
- ``web3-bzz`` swarm 协议（去中心化文件存储）相关；
- ``web3-utils`` 包含一些对 DApp 开发者有用的方法。

.. _adding-web3:

引入 web3.js
==============

.. index:: npm
.. index:: bower
.. index:: meteor

首先，需要将 web3.js 引入到项目中。 可以使用以下方法来完成：

- npm: ``npm install web3``
- meteor: ``meteor add ethereum:web3``
- pure js: link the ``dist/web3.min.js``

然后你需要创建一个 web3 的实例，设置一个 provider。
支持以太坊的浏览器如 Mist 或 MetaMask 会有提供一个 ``ethereumProvider`` 或 ``web3.currentProvider`` 。

对于 web3.js 来说，可以检查  ``Web3.givenProvider`` ，如果属性为  ``null`` 再连接本地或远程的节点。


.. code-block:: javascript

    // in node.js use: var Web3 = require('web3');

    var web3 = new Web3(Web3.givenProvider || "ws://localhost:8545");

好了，可以开始使用  ``web3`` 了。
