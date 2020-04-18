.. _eth-subscribe:

=========
web3.eth.subscribe
=========

``web3.eth.subscribe`` 方法让你可以订阅区块链中的指定事件

订阅调用
=====================

.. code-block:: javascript

    web3.eth.subscribe(type [, options] [, callback]);

----------
参数
----------

1. ``String`` - 订阅类型
2. ``Mixed`` - (可选) 依赖于订阅类型的可选额外参数
3. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为结果。该函数在每次订阅事件发生时都会被调用，订阅实例会作为第三个参数传递进来。

.. _eth-subscription-return:

-------
返回值
-------

``EventEmitter`` - 一个订阅实例

    - ``subscription.id``: 订阅 id 编号，用于标识一个订阅以及进行后续的取消订阅操作
    - ``subscription.subscribe([callback])``: 可用于使用相同的参数进行再次订阅
    - ``subscription.unsubscribe([callback])``: 取消订阅，如果成功取消的话，在回调函数中返回 `TRUE`
    - ``subscription.arguments``: 订阅参数，在重新订阅时使用
    - ``on("data")`` 返回 ``Object``: 每次有新的日志时都触发该事件，参数为日志对象
    - ``on("changed")`` 返回 ``Object``: 每次有日志从区块链上移除时触发该事件，被移除的日志对象将添加额外的属性： ``"removed: true"``
    - ``on("error")`` 返回 ``Object``: 当订阅发生错误时，触发此事件
    - ``on("connected")`` 返回 ``String``: 一旦订阅成功就会触发该事件，返回订阅 id

----------------
通知返回值
----------------

- ``Mixed`` - 取决于订阅具体是什么，可以参考文档后面提到的不同订阅获取更多相关信息

-------
例子
-------

.. code-block:: javascript

    var subscription = web3.eth.subscribe('logs', {
        address: '0x123456..',
        topics: ['0x12345...']
    }, function(error, result){
        if (!error)
            console.log(result);
    });

    // 取消订阅
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });


------------------------------------------------------------------------------


clearSubscriptions
=====================

.. code-block:: javascript

    web3.eth.clearSubscriptions()

重置订阅

.. 注意:: 这不会重置像 ``web3-shh`` 这样其它包的订阅，因为这些包有自己的请求管理器

----------
参数
----------

1. ``Boolean``: If ``true`` it keeps the ``"syncing"`` subscription.
1. ``Boolean``: 值为 ``true`` 则表示保持 ``同步`` 订阅

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.eth.subscribe('logs', {} ,function(){ ... });

    ...

    web3.eth.clearSubscriptions();


------------------------------------------------------------------------------


subscribe("pendingTransactions")
=====================

.. code-block:: javascript

    web3.eth.subscribe('pendingTransactions' [, callback]);

订阅 pending 状态的交易
----------
参数
----------

1. ``String`` - ``"pendingTransactions"``, 订阅类型
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为结果。

-------
返回值
-------

``EventEmitter``: 下面事件的一个 :ref:`订阅实例 <eth-subscription-return>` 事件发生器:

- ``"data"`` 返回 ``String``: 当接收到 pending 状态的交易时触发并返回交易哈希
- ``"error"`` 返回 ``Object``: 当订阅中发生错误时触发

----------------
通知返回值
----------------

1. ``Object|Null`` - 如果订阅失败第一个参数为错误对象
2. ``String`` - 第二个参数为交易哈希

-------
例子
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('pendingTransactions', function(error, result){
        if (!error)
            console.log(result);
    })
    .on("data", function(transaction){
        console.log(transaction);
    });

    // 取消订阅
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });


------------------------------------------------------------------------------


subscribe("newBlockHeaders")
=====================

.. code-block:: javascript

    web3.eth.subscribe('newBlockHeaders' [, callback]);

订阅新的区块头生成事件， 可用作检查链上变化的计时器。


----------
参数
----------

1. ``String`` - ``"newBlockHeaders"``, 订阅类型。
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为结果。每当有订阅事件发生时都会被调用。

-------
返回值
-------

``EventEmitter``: 带有下面事件的一个 :ref:`订阅实例 <eth-subscription-return>` 事件发生器:

- ``"data"`` 返回 ``Object``: 当收到新的区块头时触发
- ``"error"`` 返回 ``Object``: 当订阅中出现错误时触发
- ``"connected"`` 返回 ``Number``: 订阅成功后触发，返回订阅 id

返回的区块头对象结构如下：

    - ``number`` - ``Number``: 区块编号，对于 pending 状态的块该值为``null``。
    - ``hash`` 32 Bytes - ``String``: 块的哈希值，对于 pending 状态的块该值为``null``。
    - ``parentHash`` 32 Bytes - ``String``: 父区块哈希值。
    - ``nonce`` 8 Bytes - ``String``: 用来生成工作量证明的 nonce 值. 对于 pending 状态的块该值为``null``。
    - ``sha3Uncles`` 32 Bytes - ``String``: 区块中叔块数据的 sha3哈希值。
    - ``logsBloom`` 256 Bytes - ``String``: 区块日志数据的布隆过滤器。对于 pending 状态的块该值为``null``。
    - ``transactionsRoot`` 32 Bytes - ``String``: 区块中交易 trie 树的根节点。
    - ``stateRoot`` 32 Bytes - ``String``: 区块中状态 trie 树的根节点。
    - ``receiptsRoot`` 32 Bytes - ``String``: 收据根节点。
    - ``miner`` - ``String``: 接收挖矿奖励的矿工地址。
    - ``extraData`` - ``String``: 区块的额外数据字段。
    - ``gasLimit`` - ``Number``: 该块允许的最大 gas 用量。
    - ``gasUsed`` - ``Number``: 该块中所有交易使用的 gas 总量。
    - ``timestamp`` - ``Number``: 区块时间戳。

----------------
通知返回值
----------------

1. ``Object|Null`` - 如果订阅失败第一个参数为错误对象
2. ``Object`` - 像上面那样的区块头对象

-------
例子
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('newBlockHeaders', function(error, result){
        if (!error) {
            console.log(result);

            return;
        }

        console.error(error);
    })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on("data", function(blockHeader){
        console.log(blockHeader);
    })
    .on("error", console.error);

    // 取消订阅
    subscription.unsubscribe(function(error, success){
        if (success) {
            console.log('Successfully unsubscribed!');
        }
    });

------------------------------------------------------------------------------


subscribe("syncing")
=====================

.. code-block:: javascript

    web3.eth.subscribe('syncing' [, callback]);

订阅同步事件。当节点正在同步数据时将返回一个同步对象，同步结束后返回 ``FALSE``。

----------
参数
----------

1. ``String`` - ``"syncing"``, 订阅类型
2. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为结果。每当有订阅事件发生时都会被调用。

-------
返回值
-------

``EventEmitter``: 带有下面事件的一个 :ref:`订阅实例 <eth-subscription-return>` 事件发生器:

- ``"data"`` 返回 ``Object``: 收到同步对象时触发
- ``"changed"`` 返回 ``Object``: 当同步状态由 ``true`` 变为 ``false`` 时触发
- ``"error"`` 返回 ``Object``: 当订阅中出现错误时触发

要了解返回事件对象的结构，可查看 :ref:`web3.eth.isSyncing 返回值 <eth-issyncing-return>`。

----------------
通知返回值
----------------

1. ``Object|Null`` - 如果订阅失败第一个参数为错误对象
2. ``Object|Boolean`` - 同步对象, 同步开始后返回 ``true``，同步结束后返回 `false`

-------
例子
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('syncing', function(error, sync){
        if (!error)
            console.log(sync);
    })
    .on("data", function(sync){
        // 一些同步状态数据
    })
    .on("changed", function(isSyncing){
        if(isSyncing) {
            // 暂停应用操作
        } else {
            // 重启应用操作
        }
    });

    // 取消订阅
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });

------------------------------------------------------------------------------


subscribe("logs")
=====================

.. code-block:: javascript

    web3.eth.subscribe('logs', options [, callback]);

订阅日志，并按指定条件进行过滤。
如果一个有效的 ``fromBlock`` 属性被指定，Web3 会提取从这个点开始的日志，必要时回填返回值。

----------
参数
----------

1. ``"logs"`` - ``String``, 订阅类型
2. ``Object`` - 订阅选项
  - ``fromBlock`` - ``Number``: 最早区块编号， 默认为 ``null``。
  - ``address`` - ``String|Array``: 地址或地址列表，仅订阅来自这些指定账户地址的日志。
  - ``topics`` - ``Array``: 一个主题数组，数组中每个元素都应出现在日志项中。 数组中主题的顺序是很重要的, 如果你不想监听某个主题可以用 ``null`` 值, 比如 ``[null, '0x00...']``. 你也可以为每个主题传入另外一个数组来表示主题选项， 比如 ``[null, ['option1', 'option2']]``
3. ``callback`` - ``Function``: (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为结果。每当有订阅事件发生时都会被调用。

-------
返回值
-------

``EventEmitter``: 带有下面事件的一个 :ref:`订阅实例 <eth-subscription-return>` 事件发生器:

- ``"data"`` 返回 ``Object``: 接收到新日志时触发，参数为日志对象
- ``"changed"`` 返回 ``Object``: 日志从链上移除时触发，该日志同时带有附加属性 ``"removed: true"``
- ``"error"`` 返回 ``Object``: 当订阅中出现错误时触发
- ``"connected"`` 返回 ``Number``: 订阅成功后触发，返回订阅 id

要了解返回的事件对象结构，可查看 :ref:`web3.eth.getPastEvents 返回值 <eth-getpastlogs-return>`.

----------------
通知返回值
----------------

1. ``Object|Null`` - 如果订阅失败第一个参数为错误对象
2. ``Object`` - 日志对象，类似于 :ref:`web3.eth.getPastEvents 返回值 <eth-getpastlogs-return>`.

-------
例子
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('logs', {
        address: '0x123456..',
        topics: ['0x12345...']
    }, function(error, result){
        if (!error)
            console.log(result);
    })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on("data", function(log){
        console.log(log);
    })
    .on("changed", function(log){
    });

    // 取消订阅
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });
