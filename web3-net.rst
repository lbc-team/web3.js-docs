.. _net:

========
web3.*.net
========


The ``web3-net`` package allows you to interact with the Ethereum nodes network properties.
``web3-net`` 包让你可以与以太坊节点网络属性进行交互

.. code-block:: javascript

    var Net = require('web3-net');

    // "Personal.providers.givenProvider" 在支持以太坊的浏览器中会被设置.
    var net = new Net(Net.givenProvider || 'ws://some.local-or-remote.node:8546');


    // 或者使用 web3 包

    var Web3 = require('web3');
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    // -> web3.eth.net
    // -> web3.bzz.net
    // -> web3.shh.net



------------------------------------------------------------------------------


.. include:: include_package-net.rst


------------------------------------------------------------------------------
