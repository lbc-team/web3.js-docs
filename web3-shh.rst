.. _shh:

========
web3.shh
========

``web3-shh`` 包让你可以通过与 whisper 协议的交互进行消息广播。
更多相关信息可以查看 `Whisper  概述 <https://github.com/ethereum/go-ethereum/wiki/Whisper>`_.

.. code-block:: javascript

    var Shh = require('web3-shh');

    // "Shh.providers.givenProvider" 在支持以太坊的浏览器上会被设置
    var shh = new Shh(Shh.givenProvider || 'ws://some.local-or-remote.node:8546');


    // 或用 web3 包

    var Web3 = require('web3');
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    // -> web3.shh


------------------------------------------------------------------------------


.. include:: include_package-core.rst



------------------------------------------------------------------------------


.. include:: include_package-net.rst


------------------------------------------------------------------------------

getVersion
=====================

.. code-block:: javascript

    web3.shh.getVersion([callback])

返回运行中的 whisper 版本

----------
参数
----------

1. ``Function`` - (可选) 可选的回调函数，第一个参数为错误对象，第二个参数为返回结果。


-------
返回值
-------


``String`` - 当前运行中的 whisper 版本。


-------
例子
-------


.. code-block:: javascript

    web3.shh.getVersion()
    .then(console.log);
    > "5.0"


------------------------------------------------------------------------------

.. _shh-getinfo:

getInfo
=====================

.. code-block:: javascript

    web3.shh.getInfo([callback])

获取当前 whisper 节点旳信息。

----------
参数
----------

1. ``Function`` - (可选) 可选的回调函数，第一个参数为错误对象，第二个参数为返回结果。


-------
返回值
-------


``Object`` - 节点信息描述对象，具有以下属性：

    - ``messages`` - ``Number``: 当前浮动消息总数。
    - ``maxMessageSize`` - ``Number``: 当前消息大小上限，以字节为单位。
    - ``memory`` - ``Number``: 浮动消息内存大小，以字节为单位。
    - ``minPow`` - ``Number``: 当前最小 PoW 需求。


-------
例子
-------


.. code-block:: javascript

    web3.shh.getInfo()
    .then(console.log);
    > {
        "minPow": 0.8,
        "maxMessageSize": 12345,
        "memory": 1234335,
        "messages": 20
    }


------------------------------------------------------------------------------

setMaxMessageSize
=====================

.. code-block:: javascript

    web3.shh.setMaxMessageSize(size, [callback])

设置节点允许的消息大小上限。大于此限制的接收和发送消息都将被拒绝。Whisper 中的消息大小不能超过底层 P2P 协议消息大小的上限（10 Mb）。

----------
参数
----------

1. ``Number`` - 消息大小，以字节为单位。
2. ``Function`` - (可选) 可选的回调函数，第一个参数为错误对象，第二个参数为返回结果。


-------
返回值
-------


``Boolean`` - 成功则返回 true，否则返回 false。


-------
例子
-------


.. code-block:: javascript

    web3.shh.setMaxMessageSize(1234565)
    .then(console.log);
    > true


------------------------------------------------------------------------------

setMinPoW
=====================

.. code-block:: javascript

    web3.shh.setMinPoW(pow, [callback])

设置节点允许的最小 PoW 值。

该函数目前处于实验阶段，引入的目的是为了将来可以动态调整 PoW 需求。如果节点要处理大量的消息，就应该提高 PoW 需求量并通知其他节点。
要设置的新值是参照现有值表示的（比如对现有值翻倍）。现有值可以通过 :ref:`web3.shh.getInfo() <shh-getinfo>` 方法获取。

----------
参数
----------

1. ``Number`` - 新的 PoW 需求。
2. ``Function`` - (可选) 可选的回调函数，第一个参数为错误对象，第二个参数为返回结果。


-------
返回值
-------


``Boolean`` - 成功则返回 true，否则返回 false


-------
例子
-------


.. code-block:: javascript

    web3.shh.setMinPoW(0.9)
    .then(console.log);
    > true


------------------------------------------------------------------------------

markTrustedPeer
=====================

.. code-block:: javascript

    web3.shh.markTrustedPeer(enode, [callback])

将指定的节点标记为可信，从而允许其发送历史（过期）消息。

.. 注意:: 该方法不能添加新节点，参数节点必须是已经存在的。

----------
参数
----------

1. ``String`` - 可信节点的 enode 信息。
2. ``Function`` - (可选) 可选的回调函数，第一个参数为错误对象，第二个参数为返回值。


-------
返回值
-------


``Boolean`` - 成功返回 true，否则返回 false。


-------
例子
-------


.. code-block:: javascript

    web3.shh.markTrustedPeer()
    .then(console.log);
    > true


------------------------------------------------------------------------------

newKeyPair
=====================

.. code-block:: javascript

    web3.shh.newKeyPair([callback])

生成用于消息加解密的公私钥密钥对。

----------
参数
----------

1. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 密钥对生成成功则返回密钥 ID，否则返回错误信息


-------
例子
-------


.. code-block:: javascript

    web3.shh.newKeyPair()
    .then(console.log);
    > "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"


------------------------------------------------------------------------------

addPrivateKey
=====================

.. code-block:: javascript

    web3.shh.addPrivateKey(privateKey, [callback])

根据给定的私钥生成密钥对，并在保存后返回其 ID。

----------
参数
----------

1. ``String`` - 要导入的私钥，用 16 进制字符串表示
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功时返回密钥 ID，失败则返回错误信息


-------
例子
-------


.. code-block:: javascript

    web3.shh.addPrivateKey('0x8bda3abeb454847b515fa9b404cede50b1cc63cfdeddd4999d074284b4c21e15')
    .then(console.log);
    > "3e22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"


------------------------------------------------------------------------------

deleteKeyPair
=====================

.. code-block:: javascript

    web3.shh.deleteKeyPair(id, [callback])

删除指定密钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newKeyPair`` and ``shh.addPrivateKey``) 返回的密钥对 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``Boolean`` - 成功返回 true，失败返回 false


-------
例子
-------


.. code-block:: javascript

    web3.shh.deleteKeyPair('3e22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > true


------------------------------------------------------------------------------

hasKeyPair
=====================

.. code-block:: javascript

    web3.shh.hasKeyPair(id, [callback])

检查 whisper 节点的私钥是否匹配给定的 ID。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newKeyPair`` 或 ``shh.addPrivateKey``) 返回的密钥对 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------

``Boolean`` - 如果密钥对在节点中存在返回 ``true``, 否则返回 ``false``。


-------
例子
-------


.. code-block:: javascript

    web3.shh.hasKeyPair('fe22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > true


------------------------------------------------------------------------------

getPublicKey
=====================

.. code-block:: javascript

    web3.shh.getPublicKey(id, [callback])

返回指定 ID 密钥对中的公钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newKeyPair`` 或 ``shh.addPrivateKey``) 返回的密钥对 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功时返回指定密钥对的公钥，否则返回错误信息。


-------
例子
-------


.. code-block:: javascript

    web3.shh.getPublicKey('3e22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > "0x04d1574d4eab8f3dde4d2dc7ed2c4d699d77cbbdd09167b8fffa099652ce4df00c4c6e0263eafe05007a46fdf0c8d32b11aeabcd3abbc7b2bc2bb967368a68e9c6"


------------------------------------------------------------------------------

getPrivateKey
=====================

.. code-block:: javascript

    web3.shh.getPrivateKey(id, [callback])

返回指定 ID 密钥对的私钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newKeyPair`` 或 ``shh.addPrivateKey``) 返回的密钥对 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功则返回指定密钥对的私钥，否则返回错误信息。


-------
例子
-------


.. code-block:: javascript

    web3.shh.getPrivateKey('3e22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > "0x234234e22b9ffc2387e18636e0534534a3d0c56b0243567432453264c16e78a2adc"


------------------------------------------------------------------------------

newSymKey
=====================

.. code-block:: javascript

    web3.shh.newSymKey([callback])

生成随机对称密钥，保存后返回ID。
该对称秘钥在对双方都可知的情况下用于消息加解密。

----------
参数
----------

1. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功时返回密钥 ID，失败时返回错误信息。


-------
例子
-------


.. code-block:: javascript

    web3.shh.newSymKey()
    .then(console.log);
    > "cec94d139ff51d7df1d228812b90c23ec1f909afa0840ed80f1e04030bb681e4"


------------------------------------------------------------------------------

addSymKey
=====================

.. code-block:: javascript

    web3.shh.addSymKey(symKey, [callback])

保存对称密钥并返回其 ID。

----------
参数
----------

1. ``String`` - 用于对称加密的密钥，用 16 进制字符串表示
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功时返回密钥 ID，否则返回错误信息。

-------
例子
-------


.. code-block:: javascript

    web3.shh.addSymKey('0x5e11b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > "fea94d139ff51d7df1d228812b90c23ec1f909afa0840ed80f1e04030bb681e4"


------------------------------------------------------------------------------

generateSymKeyFromPassword
=====================

.. code-block:: javascript

    web3.shh.generateSymKeyFromPassword(password, [callback])

根据指定密码生成对称密钥，保存并返回其 ID。

----------
参数
----------

1. ``String`` - 用来生成对称密钥的密码。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``Promise<string>|undefined`` - 如果设置了回调函数，返回作为 Promise 的密钥 ID 或者 undefined


-------
例子
-------


.. code-block:: javascript

    web3.shh.generateSymKeyFromPassword('Never use this password - password!')
    .then(console.log);
    > "2e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"


------------------------------------------------------------------------------

hasSymKey
=====================

.. code-block:: javascript

    web3.shh.hasSymKey(id, [callback])

检查指定的 ID 是否存有对称密钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newSymKey``, ``shh.addSymKey`` 或 ``shh.generateSymKeyFromPassword``) 返回的密钥 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``Boolean`` - 如果该对称密钥在节点存在返回 ``true``, 否则返回 ``false``。


-------
例子
-------


.. code-block:: javascript

    web3.shh.hasSymKey('f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92')
    .then(console.log);
    > true


------------------------------------------------------------------------------

getSymKey
=====================

.. code-block:: javascript

    web3.shh.getSymKey(id, [callback])

返回指定 ID 关联的对称密钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newSymKey``, ``shh.addSymKey`` 或 ``shh.generateSymKeyFromPassword``) 返回的密钥 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``String`` - 成功时返回原始对称密钥，否则返回错误信息。


-------
例子
-------


.. code-block:: javascript

    web3.shh.getSymKey('af33b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > "0xa82a520aff70f7a989098376e48ec128f25f767085e84d7fb995a9815eebff0a"


------------------------------------------------------------------------------

deleteSymKey
=====================

.. code-block:: javascript

    web3.shh.deleteSymKey(id, [callback])

删除指定 ID 对应的对称密钥。

----------
参数
----------

1. ``String`` - 通过创建型函数 (``shh.newSymKey``, ``shh.addSymKey`` 或 ``shh.generateSymKeyFromPassword``) 返回的密钥 ID。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------


``Boolean`` - 对称密钥被成功删除返回 true，否则返回 false。


-------
例子
-------


.. code-block:: javascript

    web3.shh.deleteSymKey('bf31b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f')
    .then(console.log);
    > true


------------------------------------------------------------------------------


post
=====================

.. code-block:: javascript

   web3.shh.post(object [, callback])

当我们想要往网络中发送一个 whisper 消息时调用该方法。

----------
参数
----------

1. ``Object`` - 消息发送对象：
    - ``symKeyID`` - ``String`` (可选): 用于消息加密的对称秘钥 ID (``symKeyID`` 或 ``pubKey``)。
    - ``pubKey`` - ``String`` (可选): 用于消息加密的公钥 (``symKeyID`` 或 ``pubKey``)。
    - ``sig`` - ``String`` (可选): 签名密钥的 ID。
    - ``ttl`` - ``Number``: 以秒为单位的消息存活时长。
    - ``topic`` - ``String``: 4 Bytes (mandatory when key is symmetric): 4 个字节大小的消息主题（如果使用的密钥是对称密钥，该属性为必选项）。
    - ``payload`` - ``String``: 消息中要加密的内容。
    - ``padding`` - ``Number`` (可选): 对齐长度。
    - ``powTime`` - ``Number`` (可选)?: POW时间上限，以秒为单位。
    - ``powTarget`` - ``Number`` (可选)?: 消息发送对象为处理此消息需要的最小 PoW。
    - ``targetPeer`` - ``Number`` (可选): 对方节点 ID，仅用于点对点消息。
2. ``callback`` - ``Function``: (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果


-------
返回值
-------

返回 Promise 对象，在发送成功时其解析值为所发送消息的哈希值字符串，出错时将解析为错误原因。


-------
例子
-------

.. code-block:: javascript

    var identities = [];
    var subscription = null;

    Promise.all([
        web3.shh.newSymKey().then((id) => {identities.push(id);}),
        web3.shh.newKeyPair().then((id) => {identities.push(id);})

    ]).then(() => {

        // 也会收到自己的消息发送
        subscription = shh.subscribe("messages", {
            symKeyID: identities[0],
            topics: ['0xffaadd11']
        }).on('data', console.log);

    }).then(() => {
       web3.shh.post({
            symKeyID: identities[0], // 消息加密所用的密钥 ID
            sig: identities[1], // 签名消息所用的密钥对 ID
            ttl: 10,
            topic: '0xffaadd11',
            payload: '0xffffffdddddd1122',
            powTime: 3,
            powTarget: 0.5
        }).then(h => console.log(`Message with hash ${h} was successfuly sent`))
        .catch(err => console.log("Error: ", err));
    });



------------------------------------------------------------------------------


subscribe
=====================

.. code-block:: javascript

    web3.shh.subscribe('messages', options [, callback])

订阅所要接收的 whisper 消息。

.. _shh-subscribeoptions:

----------
参数
----------

1. ``"messages"`` - ``String``: 订阅类型
2. ``Object`` - 订阅选项:
    - ``symKeyID`` - ``String``: 用于消息解密的对称密钥 ID
    - ``privateKeyID`` - ``String``: 用于消息解密的私钥
    - ``sig`` - ``String`` (可选): 签名用的公钥，用于验证
    - ``topics``- ``Array`` (可选, 当使用 "privateKeyID" 时): 用于过滤消息的主题数组， 每个主题必须是 4 字节长的 16 进制字符串
    - ``minPow`` - ``Number`` (可选): 处理收到的消息所需的最小 PoW
    - ``allowP2P`` - ``Boolean`` (可选):  标示是否允许该过滤器处理直接对等消息(因为它们可能已过期，因此不再进一步转发)。这个标示在一些非常罕见的情况下才可能会用到，比如，你打算与邮件服务器通信等类似情况。
3. ``callback`` - ``Function``: (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为返回结果，每次有订阅的消息进来都会被调用，订阅对象本身会作为第三个参数传进来。


.. _shh-subscribenotificationreturns:

----------
Notification 返回值
----------

``Object`` -  接收到的消息:

    - ``hash`` - ``String``: 封装消息的哈希值
    - ``sig`` - ``String``: 消息签名对应的公钥
    - ``recipientPublicKey`` - ``String``: 接收人的公钥
    - ``timestamp`` - ``String``: 消息生成时的 unix 时间戳
    - ``ttl`` -  ``Number``: 消息存活时间，以秒为单位
    - ``topic`` - ``String``: 消息主题，4 字节的 16 进制字符串来表示
    - ``payload`` - ``String``: 解密的消息内容
    - ``padding`` - ``Number``: 可选，用来补齐长度
    - ``pow`` -  ``Number``: PoW 值


----------
例子
----------

.. code-block:: javascript

    web3.shh.subscribe('messages', {
        symKeyID: 'bf31b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f',
        sig: '0x04d1574d4eab8f3dde4d2dc7ed2c4d699d77cbbdd09167b8fffa099652ce4df00c4c6e0263eafe05007a46fdf0c8d32b11aeabcd3abbc7b2bc2bb967368a68e9c6',
        ttl: 20,
        topics: ['0xffddaa11'],
        minPow: 0.8,
    }, function(error, message, subscription){

        console.log(message);
        > {
            "hash": "0x4158eb81ad8e30cfcee67f20b1372983d388f1243a96e39f94fd2797b1e9c78e",
            "padding": "0xc15f786f34e5cef0fef6ce7c1185d799ecdb5ebca72b3310648c5588db2e99a0d73301c7a8d90115a91213f0bc9c72295fbaf584bf14dc97800550ea53577c9fb57c0249caeb081733b4e605cdb1a6011cee8b6d8fddb972c2b90157e23ba3baae6c68d4f0b5822242bb2c4cd821b9568d3033f10ec1114f641668fc1083bf79ebb9f5c15457b538249a97b22a4bcc4f02f06dec7318c16758f7c008001c2e14eba67d26218ec7502ad6ba81b2402159d7c29b068b8937892e3d4f0d4ad1fb9be5e66fb61d3d21a1c3163bce74c0a9d16891e2573146aa92ecd7b91ea96a6987ece052edc5ffb620a8987a83ac5b8b6140d8df6e92e64251bf3a2cec0cca",
            "payload": "0xdeadbeaf",
            "pow": 0.5371803278688525,
            "recipientPublicKey": null,
            "sig": null,
            "timestamp": 1496991876,
            "topic": "0x01020304",
            "ttl": 50
        }
    })
    // 或者
    .on('data', function(message){ ... });


------------------------------------------------------------------------------


clearSubscriptions
=====================

.. code-block:: javascript

    web3.shh.clearSubscriptions()

重置订阅

.. 注意:: 该调用不会重置像 ``web3-eth`` 之类其它包的订阅, 它们用它们自己的 requestManager

----------
参数
----------

1. ``Boolean``: 如果传入 ``true`` 则保持对订阅的同步

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.shh.subscribe('messages', {...} ,function(){ ... });

    ...

    web3.shh.clearSubscriptions();


------------------------------------------------------------------------------


newMessageFilter
=====================

.. code-block:: javascript

    web3.shh.newMessageFilter(options)

在节点内创建一个新的过滤器，可用于轮询满足匹配条件的新消息。


----------
参数
----------

1. ``Object``: 订阅 :ref:`web3.shh.subscribe() options <shh-subscribeoptions>` for details.

-------
返回值
-------

``String``: The filter ID.

-------
例子
-------

.. code-block:: javascript

    web3.shh.newMessageFilter()
    .then(console.log);
    > "2b47fbafb3cce24570812a82e6e93cd9e2551bbc4823f6548ff0d82d2206b326"

------------------------------------------------------------------------------


deleteMessageFilter
=====================

.. code-block:: javascript

    web3.shh.deleteMessageFilter(id)

删除节点内指定的消息过滤器。

----------
参数
----------

1. ``String``: 通过 ``shh.newMessageFilter()`` 创建的过滤器 ID

-------
返回值
-------

``Boolean``: 成功时返回 true，失败时返回 false

-------
例子
-------

.. code-block:: javascript

    web3.shh.deleteMessageFilter('2b47fbafb3cce24570812a82e6e93cd9e2551bbc4823f6548ff0d82d2206b326')
    .then(console.log);
    > true


------------------------------------------------------------------------------


getFilterMessages
=====================

.. code-block:: javascript

    web3.shh.getFilterMessages(id)

读取匹配指定过滤条件、并且在上次调用本方法之后接收到的消息。

----------
参数
----------

1. ``String``: 使用 ``shh.newMessageFilter()`` 创建的消息对象 ID

-------
返回值
-------

``Array``: 像 :ref:`web3.shh.subscribe() notification returns <shh-subscribenotificationreturns>` 这样的消息对象数组

-------
例子
-------

.. code-block:: javascript

    web3.shh.getFilterMessages('2b47fbafb3cce24570812a82e6e93cd9e2551bbc4823f6548ff0d82d2206b326')
    .then(console.log);
    > [{
        "hash": "0x4158eb81ad8e30cfcee67f20b1372983d388f1243a96e39f94fd2797b1e9c78e",
        "padding": "0xc15f786f34e5cef0fef6ce7c1185d799ecdb5ebca72b3310648c5588db2e99a0d73301c7a8d90115a91213f0bc9c72295fbaf584bf14dc97800550ea53577c9fb57c0249caeb081733b4e605cdb1a6011cee8b6d8fddb972c2b90157e23ba3baae6c68d4f0b5822242bb2c4cd821b9568d3033f10ec1114f641668fc1083bf79ebb9f5c15457b538249a97b22a4bcc4f02f06dec7318c16758f7c008001c2e14eba67d26218ec7502ad6ba81b2402159d7c29b068b8937892e3d4f0d4ad1fb9be5e66fb61d3d21a1c3163bce74c0a9d16891e2573146aa92ecd7b91ea96a6987ece052edc5ffb620a8987a83ac5b8b6140d8df6e92e64251bf3a2cec0cca",
        "payload": "0xdeadbeaf",
        "pow": 0.5371803278688525,
        "recipientPublicKey": null,
        "sig": null,
        "timestamp": 1496991876,
        "topic": "0x01020304",
        "ttl": 50
    },{...}]


