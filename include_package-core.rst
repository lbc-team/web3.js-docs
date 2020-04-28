

setProvider
=====================

.. code-block:: javascript

    web3.setProvider(myProvider)
    web3.eth.setProvider(myProvider)
    web3.shh.setProvider(myProvider)
    web3.bzz.setProvider(myProvider)
    ...

将改变相应模块的 provider。

.. note:: 当我们通过 ``web3`` 包调用它的时候，``web3.eth``, ``web3.shh`` 等子模块的 provider 也会被设置，``web3.bzz`` 这货是个例外，它总是需要一个单独的 provider。

----------
参数
----------

1. ``Object`` - ``myProvider``: :ref:`一个有效的 provider <web3-providers>`.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var Web3 = require('web3');
    var web3 = new Web3('http://localhost:8545');
    // 或
    var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

    // 改变 provider
    web3.setProvider('ws://localhost:8546');
    // 或
    web3.setProvider(new Web3.providers.WebsocketProvider('ws://localhost:8546'));

    // 在 node.js 中使用 IPC provider
    var net = require('net');
    var web3 = new Web3('/Users/myuser/Library/Ethereum/geth.ipc', net); // mac os 路径
    // 或
    var web3 = new Web3(new Web3.providers.IpcProvider('/Users/myuser/Library/Ethereum/geth.ipc', net)); // mac os 路径
    // 在 windows 中路径为: "\\\\.\\pipe\\geth.ipc"
    // 在 linux 中路径为: "/users/myuser/.ethereum/geth.ipc"


------------------------------------------------------------------------------

providers
=====================

.. code-block:: javascript

    web3.providers
    web3.eth.providers
    web3.shh.providers
    web3.bzz.providers
    ...

包含当前可用的 :ref:`providers <web3-providers>`.

----------
返回值
----------

具有以下 provider 的 ``Object``:

    - ``Object`` - ``HttpProvider``: The HTTP provider is **deprecated**, as it won't work for subscriptions.
    - ``Object`` - ``WebsocketProvider``: The Websocket provider is the standard for usage in legacy browsers.
    - ``Object`` - ``IpcProvider``: The IPC provider is used node.js dapps when running a local node. Gives the most secure connection.
    - ``Object`` - ``HttpProvider``: 因为不能用于订阅，HTTP provider 已经**不推荐使用**。
    - ``Object`` - ``WebsocketProvider``: Websocket provider 是用于传统的浏览器中的标准方法.
    - ``Object`` - ``IpcProvider``: 当运行一个本地节点时，IPC provider 用于 node.js 下的D App 环境，提供最为安全的连接。.

-------
例子
-------

.. code-block:: javascript

    var Web3 = require('web3');
    // 使用指定的 Provider （e.g 比如在 Mist 中） 或者实例化一个新的 websocket provider
    var web3 = new Web3(Web3.givenProvider || 'ws://remotenode.com:8546');
    // 或者
    var web3 = new Web3(Web3.givenProvider || new Web3.providers.WebsocketProvider('ws://remotenode.com:8546'));

    // 在 node.js 中使用 IPC provider
    var net = require('net');

    var web3 = new Web3('/Users/myuser/Library/Ethereum/geth.ipc', net); // mac os 路径
    // 或者
    var web3 = new Web3(new Web3.providers.IpcProvider('/Users/myuser/Library/Ethereum/geth.ipc', net)); // mac os 路径
    // windows 上的路径为: "\\\\.\\pipe\\geth.ipc"
    // linux 上的路径为: "/users/myuser/.ethereum/geth.ipc"


------------------------------------------------------------------------------

givenProvider
=====================

.. code-block:: javascript

    web3.givenProvider
    web3.eth.givenProvider
    web3.shh.givenProvider
    web3.bzz.givenProvider
    ...

在和以太坊兼容的浏览器中使用 web3.js 时，当前环境的原生 provider 会被浏览器设置。
web3.givenProvider 将返回浏览器设置的原生 provider ，否则返回 ``null``。

-------
返回值
-------

``Object``: 浏览器设置好的 provider 或者 ``null``;

-------
例子
-------

.. code-block:: javascript
    web3.setProvider(web3.givenProvider || "ws://remotenode.com:8546");

------------------------------------------------------------------------------


currentProvider
=====================

.. code-block:: javascript

    web3.currentProvider
    web3.eth.currentProvider
    web3.shh.currentProvider
    web3.bzz.currentProvider
    ...

-------
返回值
-------

``Object``: 当前在用的 provider 或者 ``null``;

-------
例子
-------

.. code-block:: javascript
    if(!web3.currentProvider) {
        web3.setProvider("http://localhost:8545");
    }

------------------------------------------------------------------------------

BatchRequest
=====================

.. code-block:: javascript

    new web3.BatchRequest()
    new web3.eth.BatchRequest()
    new web3.shh.BatchRequest()
    new web3.bzz.BatchRequest()

用来创建并执行批量请求的类

----------
参数
----------

none

-------
返回值
-------

``Object``: 具有如下方法的一个对象:

    - ``add(request)``: To add a request object to the batch call.
    - ``execute()``: Will execute the batch request.
    - ``add(request)``: 添加请求对象到批量调用中。
    - ``execute()``: 执行批量请求。

-------
例子
-------

.. code-block:: javascript

    var contract = new web3.eth.Contract(abi, address);

    var batch = new web3.BatchRequest();
    batch.add(web3.eth.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
    batch.add(contract.methods.balance(address).call.request({from: '0x0000000000000000000000000000000000000000'}, callback2));
    batch.execute();


------------------------------------------------------------------------------

extend
=====================

.. code-block:: javascript

    web3.extend(methods)
    web3.eth.extend(methods)
    web3.shh.extend(methods)
    web3.bzz.extend(methods)
    ...

用来扩展 web3 模块

.. note:: 你也可以使用 ``*.extend.formatters`` 作为额外的格式化函数进行输入输出参数的格式化. 更多详情请看 `源文件 <https://github.com/ethereum/web3.js/blob/master/packages/web3-core-helpers/src/formatters.js>`_ 。

----------
参数
----------

1. ``methods`` - ``Object``: 扩展对象，带有一组如下所示的方法描述对象:
    - ``property`` - ``String``: (可选) 要添加到模块上的属性名称。如果没有设置属性，则直接添加到模块上。
    - ``methods`` - ``Array``: 方法描述对象数组：
        - ``name`` - ``String``: 要添加的方法名称。
        - ``call`` - ``String``: RPC 方法名称。
        - ``params`` - ``Number``: (可选) 方法的参数个数，默认值为 0。
        - ``inputFormatter`` - ``Array``: (可选) 输入格式化函数数组，每个成员对应一个函数参数，或者使用 null 来对应不需要进行格式化处理的参数。
        - ``outputFormatter - ``Function``: (可选) 用来格式化方法输出。

----------
返回值
----------

``Object``: 扩展模块.

-------
例子
-------

.. code-block:: javascript

    web3.extend({
        property: 'myModule',
        methods: [{
            name: 'getBalance',
            call: 'eth_getBalance',
            params: 2,
            inputFormatter: [web3.extend.formatters.inputAddressFormatter, web3.extend.formatters.inputDefaultBlockNumberFormatter],
            outputFormatter: web3.utils.hexToNumberString
        },{
            name: 'getGasPriceSuperFunction',
            call: 'eth_gasPriceSuper',
            params: 2,
            inputFormatter: [null, web3.utils.numberToHex]
        }]
    });

    web3.extend({
        methods: [{
            name: 'directCall',
            call: 'eth_callForFun',
        }]
    });

    console.log(web3);
    > Web3 {
        myModule: {
            getBalance: function(){},
            getGasPriceSuperFunction: function(){}
        },
        directCall: function(){},
        eth: Eth {...},
        bzz: Bzz {...},
        ...
    }


------------------------------------------------------------------------------
