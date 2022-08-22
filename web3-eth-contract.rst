.. _eth-contract:

====================
web3.eth.Contract
====================

``web3.eth.Contract`` 对象让你可以轻松地与以太坊区块链上的智能合约进行交互。
当你创建一个新的合约对象时，只需要指定相应的智能合约 json 接口， web3 就会自动将所有的调用转换为基于 RPC 的底层 ABI 调用。

这使得你可以像 JavaScript 对象一样与智能合约进行交互。

要单独使用它：

.. code-block:: javascript
    var Contract = require('web3-eth-contract');

    // 设置 provider 以便为后面所有的实例所用
    Contract.setProvider('ws://localhost:8546');

    var contract = new Contract(jsonInterface, address);

    contract.methods.somFunc().send({from: ....})
    .on('receipt', function(){
        ...
    });


------------------------------------------------------------------------------


new contract
==================

.. index:: JSON interface

.. code-block:: javascript

    new web3.eth.Contract(jsonInterface[, address][, options])

创建新的合约实例，并在其 :ref:`json interface <glossary-json-interface>` 对象中定义所有的方法和事件。

----------
参数
----------

1. ``jsonInterface`` - ``Object``: 所要实例化合约的 json 接口
2. ``address`` - ``String`` （可选）: 要调用的智能合约地址
3. ``options`` - ``Object`` （可选）: 合约配置选项。 其中某些选项被用作合约调用和交易的回调:
    * ``from`` - ``String``: 交易发送方地址
    * ``gasPrice`` - ``String``: 为交易指定的 gas 价格，以 wei 为单位
    * ``gas`` - ``Number``: 交易可用的最大 gas 量（gas limit）。
    * ``data`` - ``String``: 合约字节码。当合约被 :ref:`部署 <contract-deploy>`时需要使用。

----------
返回值
----------

``Object``: 带有所有方法和事件的合约实例


-------
例子
-------

.. code-block:: javascript

    var myContract = new web3.eth.Contract([...], '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe', {
        from: '0x1234567890123456789012345678901234567891', // 默认交易发送地址
        gasPrice: '20000000000' // 以 wei 为单位的默认 gas 价格，当前价格为 20 gwei
    });


------------------------------------------------------------------------------


= 属性列表 =
============

------------------------------------------------------------------------------

.. _eth-contract-defaultaccount

defaultAccount
=====================

.. code-block:: javascript

    web3.eth.Contract.defaultAccount
    contract.defaultAccount // 合约实例上的默认账户

如果下面这些方法没有指定 ``"from"`` 属性，默认账户地址就会被用作默认的 ``"from"`` 属性：

- :ref:`web3.eth.sendTransaction() <eth-sendtransaction>`
- :ref:`web3.eth.call() <eth-call>`
- :ref:`new web3.eth.Contract() -> myContract.methods.myMethod().call() <eth-contract-call>`
- :ref:`new web3.eth.Contract() -> myContract.methods.myMethod().send() <eth-contract-send>`

--------
属性
--------


``String`` - 20 字节: 任意以太坊地址。 你应该在你的节点或 keystore 中拥有该地址的私钥。 (默认值为 ``undefined``)


-------
例子
-------


.. code-block:: javascript

    web3.eth.defaultAccount;
    > undefined

    // 设置默认账户
    web3.eth.defaultAccount = '0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe';


------------------------------------------------------------------------------

.. _eth-contract-defaultblock:

defaultBlock
=====================

.. code-block:: javascript

    web3.eth.Contract.defaultBlock
    contract.defaultBlock // 在合约实例上

默认块在一些特定方法上使用。你可以通过传入 defaultBlock 作为最后一个参数来覆盖它。
默认值为 "latest"。

----------
属性
----------



默认块参数的可能取值如下：

- ``Number|BN|BigNumber``: 区块号
- ``"genesis"`` - ``String``: 创世块
- ``"latest"`` - ``String``: 最新块（也就是当前链头块）
- ``"pending"`` - ``String``: 正在挖的块（包括 pending 状态交易）
- ``"earliest"`` - ``String``: 创世块

默认值为 ``"latest"``


-------
例子
-------

.. code-block:: javascript

    contract.defaultBlock;
    > "latest"

    // 设置默认块
    contract.defaultBlock = 231;



------------------------------------------------------------------------------

.. _eth-contract-defaulthardfork:

defaultHardfork
=====================

.. code-block:: javascript

    contract.defaultHardfork

本地签名交易时用的默认硬分叉属性

----------
属性
----------


默认硬分叉属性的可能取值如下：

- ``"chainstart"`` - ``String``
- ``"homestead"`` - ``String``
- ``"dao"`` - ``String``
- ``"tangerineWhistle"`` - ``String``
- ``"spuriousDragon"`` - ``String``
- ``"byzantium"`` - ``String``
- ``"constantinople"`` - ``String``
- ``"petersburg"`` - ``String``
- ``"istanbul"`` - ``String``

默认值为 ``"petersburg"``

-------
例子
-------

.. code-block:: javascript

    contract.defaultHardfork;
    > "petersburg"

    // 设置默认硬分叉
    contract.defaultHardfork = 'istanbul';


------------------------------------------------------------------------------

.. _eth-contract-defaultchain:

defaultChain
=====================

.. code-block:: javascript

    contract.defaultChain

默认链属性是本地签名交易的时候用的

----------
属性
----------


默认链属性可以是一下列表中之一：

- ``"mainnet"`` - ``String``
- ``"goerli"`` - ``String``
- ``"kovan"`` - ``String``
- ``"rinkeby"`` - ``String``
- ``"ropsten"`` - ``String``

默认值为 ``"mainnet"``

-------
例子
-------

.. code-block:: javascript

    contract.defaultChain;
    > "mainnet"

    // 设置默认链
    contract.defaultChain = 'goerli';


------------------------------------------------------------------------------

.. _eth-contract-defaultcommon:

defaultCommon
=====================

.. code-block:: javascript

    contract.defaultCommon

默认通用属性是本地签名交易的时候用的

----------
属性
----------


默认通用属性包含如下所示的  ``Common`` 对象：

- ``customChain`` - ``Object``: 自定义链属性
    - ``name`` - ``string``: （可选） 链名字
    - ``networkId`` - ``number``: 自定义链的网络 Id
    - ``chainId`` - ``number``: 自定义链的链 Id
- ``baseChain`` - ``string``: （可选） ``mainnet``, ``goerli``, ``kovan``, ``rinkeby``, or ``ropsten``
- ``hardfork`` - ``string``: （可选） ``chainstart``, ``homestead``, ``dao``, ``tangerineWhistle``, ``spuriousDragon``, ``byzantium``, ``constantinople``, ``petersburg``, or ``istanbul``


默认值为 ``undefined``。

-------
例子
-------

.. code-block:: javascript

    contract.defaultCommon;
    > {customChain: {name: 'custom-network', chainId: 1, networkId: 1}, baseChain: 'mainnet', hardfork: 'petersburg'}

    // 设置默认通用值
    contract.defaultCommon = {customChain: {name: 'custom-network', chainId: 1, networkId: 1}, baseChain: 'mainnet', hardfork: 'petersburg'};


------------------------------------------------------------------------------

.. _eth-contract-transactionblocktimeout:

transactionBlockTimeout
=============================

.. code-block:: javascript

    web3.eth.Contract.transcationBlockTimeout
    contract.transactionBlockTimeout // 在合约实例上

``transactionBlockTimeout`` 会被用在基于套接字的连接上。该属性定义了直到第一次确认发生它应该等待的区块数。
这意味着当超时发生时，PromiEvent 会拒绝并显示超时错误。

----------
返回值
----------

``number``: transactionBlockTimeout 当前设定的值 (默认值: 50)

------------------------------------------------------------------------------

.. _eth-contract-module-transactionconfirmationblocks:

transactionConfirmationBlocks
===============================

.. code-block:: javascript

    web3.eth.Contract.transactionConfirmationBlocks
    contract.transactionConfirmationBlocks // 在合约实例上

定义了确认交易所需要的区块确认数。

----------
返回值
----------

``number``: transactionConfirmationBlocks 当前设定的值 (默认值: 24)

------------------------------------------------------------------------------

.. _eth-contract-module-transactionpollingtimeout:

transactionPollingTimeout
===========================

.. code-block:: javascript

    web3.eth.Contract.transactionPollingTimeout
    contract.transactionPollingTimeout // 在合约实例上

``transactionPollingTimeout`` 将通过 HTTP 连接使用。
这个选项定义了 Web3 等待确认交易被网络挖出的收据的秒数。注意：当此种超时发生时，交易可能仍未完成。

----------
返回值
----------

``number``: transactionPollingTimeout 当前设定的值 (默认值: 750)

------------------------------------------------------------------------------

.. _eth-contract-module-handlerevert:

handleRevert
=============

.. code-block:: javascript

    web3.eth.Contract.handleRevert
    contract.handleRevert // 在合约实例上

``handleRevert`选项属性默认值为`false``，如果在 :ref:`send <contract-send>`或者 :ref:`call <contract-call>`合约方法时启用，将返回回退原因字符串。

.. note:: 回退原因字符串和签名是存在于返回错误的属性中的。

----------
返回值
----------

``boolean``: ``handleRevert`` 的当前值 (默认值: false)

------------------------------------------------------------------------------

选项属性
=========

.. code-block:: javascript

    myContract.options

合约实例的选项属性 ``对象``。``from``、 ``gas`` 和 ``gasPrice``作为发送交易时的备用值使用。

-------
属性
-------

``Object`` - 选项属性:

- ``address`` - ``String``: 合约的部署地址。 见 :ref:`options.address <contract-address>`.
- ``jsonInterface`` - ``Array``: 合同的 json 接口。见 :ref:`options.jsonInterface <contract-json-interface>`.
- ``data`` - ``String``: 合约字节码。 :ref:`部署 <contract-deploy>` 合约时使用。
- ``from`` - ``String``: 交易发起方地址。
- ``gasPrice`` - ``String``: 用于交易的 gas 价格。以 wei 为单位。
- ``gas`` - ``Number``: 可用于该交易的 gas 用量上限 (gas limit)。
- ``handleRevert`` - ``Boolean``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`handleRevert <eth-contract-module-handlerevert>`.
- ``transactionBlockTimeout`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`transactionBlockTimeout <eth-contract-transactionblocktimeout>`.
- ``transactionConfirmationBlocks`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`transactionConfirmationBlocks <eth-contract-module-transactionconfirmationblocks>`.
- ``transactionPollingTimeout`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`transactionPollingTimeout <eth-contract-module-transactionpollingtimeout>`.
- ``chain`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`defaultChain <eth-contract-defaultchain>`.
- ``hardfork`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`defaultHardfork <eth-contract-defaulthardfork>`.
- ``common`` - ``Number``: 如果这里不设置，将使用 Eth 模块提供的默认值。 见 :ref:`defaultCommon <eth-contract-defaultcommon>`.


-------
例子
-------

.. code-block:: javascript

    myContract.options;
    > {
        address: '0x1234567890123456789012345678901234567891',
        jsonInterface: [...],
        from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
        gasPrice: '10000000000000',
        gas: 1000000
    }

    myContract.options.from = '0x1234567890123456789012345678901234567891'; // 默认交易发送方地址
    myContract.options.gasPrice = '20000000000000'; // 默认 gas 价格，以 wei 为单位
    myContract.options.gas = 5000000; // 5M gas 作为备用值


------------------------------------------------------------------------------

.. _contract-address:

options.address
==================

.. code-block:: javascript

    myContract.options.address

用于本合约实例的地址。
所有通过 web3.js 从这个合约生成的交易都将包含这个地址作为 "to" 地址（也就是交易接收方地址）。

地址将以小写的方式保存。

-------
属性
-------

``address`` - ``String|null``: 本合约的地址, 如果未设置其值为 ``null``。


-------
例子
-------

.. code-block:: javascript

    myContract.options.address;
    > '0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae'

    // 设置新地址
    myContract.options.address = '0x1234FFDD...';


------------------------------------------------------------------------------

.. _contract-json-interface:

options.jsonInterface
===========================

.. code-block:: javascript

    myContract.options.jsonInterface

从合约的 `ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`_ 派生出来的 :ref:`json 接口 <glossary-json-interface>` 对象。

-------
属性
-------

``jsonInterface`` - ``Array``: 当前合约的 :ref:`json 接口 <glossary-json-interface>` 。重设该接口会导致合约实例方法和事件的重新生成。


-------
例子
-------

.. code-block:: javascript

    myContract.options.jsonInterface;
    > [{
        "type":"function",
        "name":"foo",
        "inputs": [{"name":"a","type":"uint256"}],
        "outputs": [{"name":"b","type":"address"}]
    },{
        "type":"event",
        "name":"Event",
        "inputs": [{"name":"a","type":"uint256","indexed":true},{"name":"b","type":"bytes32","indexed":false}],
    }]

    // 设置新的接口
    myContract.options.jsonInterface = [...];


------------------------------------------------------------------------------


= 方法 =
=========


------------------------------------------------------------------------------

clone
=====================

.. code-block:: javascript

    myContract.clone()

克隆当前合约实例。

----------
参数
----------

无

----------
返回值
----------


``Object``: 新合约实例。

-------
例子
-------

.. code-block:: javascript

    var contract1 = new eth.Contract(abi, address, {gasPrice: '12345678', from: fromAddress});

    var contract2 = contract1.clone();
    contract2.options.address = address2;

    (contract1.options.address !== contract2.options.address);
    > true

------------------------------------------------------------------------------


.. _contract-deploy:

.. index:: contract deploy

deploy
=====================

.. code-block:: javascript

    myContract.deploy(options)

调用此函数将合约部署到区块链上。
成功部署后 promise 对象会被解析为新的合约实例。

----------
参数
----------

1. ``options`` - ``Object``: 用于合约部署的选项。
    * ``data`` - ``String``: 合约字节码
    * ``arguments`` - ``Array`` （可选）: 在部署合约时传递给构造函数的参数。

----------
返回值
----------


``Object``: 交易对象：

- ``Array`` - 参数: 之前传递给方法的参数。它们是可以被改变的。
- ``Function`` - :ref:`send <contract-send>`: 用来部署合约。promise 会被解析为合约实例而不是交易收据。
- ``Function`` - :ref:`estimateGas <contract-estimateGas>`: 估算部署合约所需要的 gas 用量。
- ``Function`` - :ref:`encodeABI <contract-encodeABI>`: 编码由合约数据和构造函数参数构成的合约部署 ABI。

关于这些方法的更多详情，参见下面的文档。

-------
例子
-------

.. code-block:: javascript

    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .send({
        from: '0x1234567890123456789012345678901234567891',
        gas: 1500000,
        gasPrice: '30000000000000'
    }, function(error, transactionHash){ ... })
    .on('error', function(error){ ... })
    .on('transactionHash', function(transactionHash){ ... })
    .on('receipt', function(receipt){
       console.log(receipt.contractAddress) // 包含新合约地址
    })
    .on('confirmation', function(confirmationNumber, receipt){ ... })
    .then(function(newContractInstance){
        console.log(newContractInstance.options.address) // 带有新合约地址的合约实例
    });


    //数据已经设置为合约本身的选项
    myContract.options.data = '0x12345...';

    myContract.deploy({
        arguments: [123, 'My String']
    })
    .send({
        from: '0x1234567890123456789012345678901234567891',
        gas: 1500000,
        gasPrice: '30000000000000'
    })
    .then(function(newContractInstance){
        console.log(newContractInstance.options.address) // 具有新合同地址的合约实例
    });


    // 只是编码
    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .encodeABI();
    > '0x12345...0000012345678765432'


    // gas 估算
    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .estimateGas(function(err, gas){
        console.log(gas);
    });

------------------------------------------------------------------------------


methods
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]])

为指定方法创建一个交易对象, 可以被用来 :ref:`called <contract-call>`, :ref:`send <contract-send>`, :ref:`estimated  <contract-estimateGas>`或者 :ref:`ABI encoded <contract-encodeABI>`.

该智能合约的方法可以通过下面几种方式得到:

- 方法名: ``myContract.methods.myMethod(123)``
- 带参数的方法名: ``myContract.methods['myMethod(uint256)'](123)``
- 方法签名: ``myContract.methods['0x58cf5f10'](123)``

这样可以调用与 JavaScript 合约对象名称相同但参数不同的函数。

----------
参数
----------

任何方法的参数都取决于在 :ref:`JSON 接口 <glossary-json-interface>` 中定义的智能合约方法。

----------
返回值
----------

``Object``: 交易对象:

- ``Array`` - 参数: 之前传递给方法的参数。它们是可以被改变的。
- ``Function`` - :ref:`call <contract-call>`: 将在不发送交易的情况下调用该“常量”方法并在 EVM 中执行其智能合约方法（无法更改智能合约状态）。
- ``Function`` - :ref:`send <contract-send>`: 用来向合约发送交易并执行其方法（可以更改智能合约状态）。
- ``Function`` - :ref:`estimateGas <contract-estimateGas>`: 将估算在链上执行该方法时所要消耗的 gas。
- ``Function`` - :ref:`encodeABI <contract-encodeABI>`: 对合于方法进行 ABI 编码。得到的编码可以通过交易去 send，可以直接 call，也可以作为参数传给另外一个智能合约方法，

关于这些方法的更多详情，参见下面的文档。

-------
例子
-------

.. code-block:: javascript

    // 对一个方法发起 call 调用

    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, result){
        ...
    });

    // 或者发生交易并使用 promise
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(receipt){
        // receipt can also be a new contract instance, when coming from a "contract.deploy({...}).send()"
    });

    // 或者发送交易并使用事件
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .on('transactionHash', function(hash){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('error', function(error, receipt) {
        ...
    });


------------------------------------------------------------------------------


.. _contract-call:

methods.myMethod.call
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).call(options[, callback])

将在不发送交易的情况下调用该“常量”方法并在 EVM 中执行其智能合约方法。注意此种调用方式无法改变智能合约状态。

----------
参数
----------

1. ``options`` - ``Object`` （可选）: 用于发起调用的选项。
    * ``from`` - ``String`` （可选）: 调用“交易”的发起方地址。
    * ``gasPrice`` - ``String`` （可选）: 用于该调用“交易”的 gas 价格。以 wei 为计量单位。
    * ``gas`` - ``Number`` （可选）: 用于该调用“交易”的 gas 用量上限 (gas limit)。
2. ``callback`` - ``Function`` （可选）: 回调函数，回调时将以智能合约方法执行结果作为第二个参数，以错误对象作为第一个参数。

----------
返回值
----------

``Promise`` 返回 ``Mixed``: 智能合约方法返回值。
如果只返回一个值，则按原样返回。如果有多个返回值，则作为一个带有属性和索引的对象返回。

-------
例子
-------

.. code-block:: javascript

    // 使用回调
    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, result){
        ...
    });

    // 使用 promise
    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(result){
        ...
    });


    // 多返回值:

    // Solidity
    contract MyContract {
        function myFunction() returns(uint256 myNumber, string myString) {
            return (23456, "Hello!%");
        }
    }

    // web3.js
    var MyContract = new web3.eth.Contract(abi, address);
    MyContract.methods.myFunction().call()
    .then(console.log);
    > Result {
        myNumber: '23456',
        myString: 'Hello!%',
        0: '23456', // 这里这些是作为属性名称时的备用值
        1: 'Hello!%'
    }


    // 单参数返回:

    // Solidity
    contract MyContract {
        function myFunction() returns(string myString) {
            return "Hello!%";
        }
    }

    // web3.js
    var MyContract = new web3.eth.Contract(abi, address);
    MyContract.methods.myFunction().call()
    .then(console.log);
    > "Hello!%"



------------------------------------------------------------------------------


.. _contract-send:

methods.myMethod.send
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).send(options[, callback])

向合约发送交易来执行其方法。注意这会改变合约状态。

----------
参数
----------

1. ``options`` - ``Object``: 用来发送交易的选项。
    * ``from`` - ``String``: 交易发送方地址。
    * ``gasPrice`` - ``String`` （可选）: 用于该交易的 gas 价格，以 wei 为单位。
    * ``gas`` - ``Number`` （可选）: 该交易 gas 用量上限 (gas limit)。
    * ``value`` - ``Number|String|BN|BigNumber``（可选）: 交易转账金额，以 wei 为单位。
2. ``callback`` - ``Function`` （可选）: 回调函数，触发时其第二个参数为交易哈希，第一个参数为错误对象。

----------
返回值
----------

**回调** 将返回 32 字节的交易哈希值。

``PromiEvent``: 一个 :ref:`整合了事件发生器 <promiEvent> promise`. 当收到交易*收据*时会被解析, 当该``send()``调用是从``someContract.deploy()``发出时，promise 会解析为*新合约地址*。重设该接口会导致合约实例方法和事件的重新生成。 此外也存在下面这些事件:

- ``"transactionHash"`` 返回 ``String``: 发送交易且得到交易哈希值后立即触发。
- ``"receipt"`` 返回 ``Object``: 当收到交易*收据*时触发。合约收据带有的不是``logs``，而是以事件名称为健，以事件本身为属性值的``events``。 关于返回事件对象的详情，参见 :ref:`getPastEvents 返回值 <contract-events-return>` 。
- ``"confirmation"`` 返回 ``Number``, ``Object``: 从区块被挖到的第一个区块确认开始，每次确认都会触发，直到第 24 次确认。触发时第一个参数为收到的确认数，第二个参数为收到交易收据。 
- ``"error"`` 返回 ``Error`` 和 ``Object|undefined``: 交易发送过程中出错时触发。如果交易被网络拒绝且带有交易收据，第二个参数就是该交易收据。


-------
例子
-------

.. code-block:: javascript

    // 使用回调
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, transactionHash){
        ...
    });

    // 使用 promise
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(receipt){
        // 当这个 receipt 对象来自于 "contract.deploy({...}).send()" 这么个调用时，它也可以是一个新合约实例
    });


    // 使用事件触发器
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        // receipt 相关例子
        console.log(receipt);
        > {
            "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
            "transactionIndex": 0,
            "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
            "blockNumber": 3,
            "contractAddress": "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe",
            "cumulativeGasUsed": 314159,
            "gasUsed": 30234,
            "events": {
                "MyEvent": {
                    returnValues: {
                        myIndexedParam: 20,
                        myOtherIndexedParam: '0x123456789...',
                        myNonIndexParam: 'My String'
                    },
                    raw: {
                        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
                        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
                    },
                    event: 'MyEvent',
                    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                    logIndex: 0,
                    transactionIndex: 0,
                    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
                    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                    blockNumber: 1234,
                    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
                },
                "MyOtherEvent": {
                    ...
                },
                "MyMultipleEvent":[{...}, {...}] // 如果同一事件有多个，则它们将被组装到数组中
            }
        }
    })
    .on('error', function(error, receipt) { // 如果交易被网络拒绝并带有交易收据，则第二个参数将是交易收据。
        ...
    });

------------------------------------------------------------------------------


.. _contract-estimateGas:

methods.myMethod.estimateGas
==============================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).estimateGas(options[, callback])

通过在 EVM 中执行方法来估算链上执行是需要的 gas 用量。
由于彼时合约状态的不同，当前估算的 gas 用量和随后通过真实交易所得到的实际 gas 用量可能会有所出入。

----------
参数
----------

1. ``options`` - ``Object`` （可选）: 用于调用的选项。
    * ``from`` - ``String`` （可选）: 交易发起方地址。
    * ``gas`` - ``Number`` （可选）: 交易 gas 用量上限 (gas limit)。设置特定的值有助于检测 gas 耗尽相关错误，gas 耗尽时会返回相同的值。
    * ``value`` - ``Number|String|BN|BigNumber`` （可选）: 交易转账金额，以 wei 为单位。
2. ``callback`` - ``Function`` （可选）: 回调函数，触发时其第二个参数为 gas 估算量，第一个参数为错误对象。

----------
返回值
----------

``Promise`` 返回 ``Number``: 估算的 gas 用量

-------
例子
-------

.. code-block:: javascript

    // 使用回调
    myContract.methods.myMethod(123).estimateGas({gas: 5000000}, function(error, gasAmount){
        if(gasAmount == 5000000)
            console.log('Method ran out of gas');
    });

    // 使用 promise
    myContract.methods.myMethod(123).estimateGas({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(gasAmount){
        ...
    })
    .catch(function(error){
        ...
    });


------------------------------------------------------------------------------


.. _contract-encodeABI:

methods.myMethod.encodeABI
=============================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).encodeABI()

为指定的合约方法进行 ABI 编码，可用于发送交易、调用方法或向另一个合约方法传递参数。


----------
参数
----------

无

----------
返回值
----------

``String``: 编码后的 ABI 字节码，可用于交易发送或方法调用。

-------
例子
-------

.. code-block:: javascript

    myContract.methods.myMethod(123).encodeABI();
    > '0x58cf5f1000000000000000000000000000000000000000000000000000000000000007B'


------------------------------------------------------------------------------


= 事件 =
=========


------------------------------------------------------------------------------


once
=====================

.. code-block:: javascript

    myContract.once(event[, options], callback)

订阅一个事件并在第一次事件触发或错误发生后立即取消订阅。一个事件仅触发一次。

----------
参数
----------

1. ``event`` - ``String``: 要订阅的合约事件名, 或者用 ``"allEvents"`` 来订阅所有事件。
2. ``options`` - ``Object`` （可选）: 用于部署的选项。
    * ``filter`` - ``Object`` （可选）: 按索引参数过滤事件, 例如 ``{filter: {myNumber: [12,13]}}`` 表示 "myNumber" 为 12 或 13 的所有事件。
    * ``topics`` - ``Array`` （可选）: 手动设置事件过滤器的主题。如果提供了过滤器属性和事件签名，则不会自动设置（topic [0]）
3. ``callback`` - ``Function``: 回调函数，触发时把*事件*对象作为第二个参数，错误作为第一个参数。 关于详细事件结构，参见 :ref:`getPastEvents 返回值 <contract-events-return>` 。

----------
返回值
----------

``undefined``

-------
例子
-------

.. code-block:: javascript

    myContract.once('MyEvent', {
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // 使用数组表示 或：比如 20 或 23。
        fromBlock: 0
    }, function(error, event){ console.log(event); });

    // 事件输入的例子
    > {
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }


------------------------------------------------------------------------------

.. _contract-events:

events
=====================

.. code-block:: javascript

    myContract.events.MyEvent([options][, callback])

订阅指定的合约事件

----------
参数
----------

1. ``options`` - ``Object`` （可选）: 用于部署的选项。
    * ``filter`` - ``Object`` （可选）: 按索引参数过滤事件, 例如 ``{filter: {myNumber: [12,13]}}`` 表示 "myNumber" 为 12 或 13 的所有事件。
    * ``fromBlock`` - ``Number|String|BN|BigNumber`` （可选）: 读取从该编号开始的区块中的事件（大于或等于该区块号）。 也可以使用预先定义的区块号，比如 ``"latest"``, ``"earliest"``, ``"pending"``, ``"genesis"`` 等。
    * ``topics`` - ``Array`` （可选）: 手动设置事件过滤器的主题。如果提供了过滤器属性和事件签名，则不会自动设置（topic [0]）。
2. ``callback`` - ``Function`` （可选）: 回调函数，触发时把*事件*对象作为第二个参数，错误作为第一个参数。

.. _contract-events-return:

----------
返回值
----------

``EventEmitter``: 该事件发生器有以下事件：

- ``"data"`` 返回 ``Object``: 接收到新传入的事件时触发，参数为事件对象。
- ``"changed"`` 返回 ``Object``: 当事件从区块链上移除时触发。 该事件带有额外属性 ``"removed: true"``。
- ``"error"`` 返回 ``Object``: 当订阅中出现错误时触发。
- ``"connected"`` 返回 ``String``: 当订阅成功连接时触发一次。返回订阅 id。


返回的事件 "对象" 结构如下：

- ``event`` - ``String``: 事件名称。
- ``signature`` - ``String|Null``: 事件签名，如果是匿名事件，其值为 ``null``。
- ``address`` - ``String``: 该事件的发源地地址。
- ``returnValues`` - ``Object``: 事件返回值， 比如 ``{myVar: 1, myVar2: '0x234...'}``.
- ``logIndex`` - ``Number``: 事件在区块中的索引位置。
- ``transactionIndex`` - ``Number``: 事件所在交易在区块中的索引位置。
- ``transactionHash`` 32 Bytes - ``String``: 事件所在交易的哈希值。
- ``blockHash`` 32 Bytes - ``String``: 事件所在区块链的哈希值。区块处于 pending 状态时其值为 ``null`` 。
- ``blockNumber`` - ``Number``: 事件所在区块的区块号。 区块处于 pending 状态时其值为 ``null`` 。
- ``raw.data`` - ``String``: 包含未索引的日志参数。
- ``raw.topics`` - ``Array``: 最大可保存 4 个 32 字节长的主题数组，主题 1-3 包含事件的索引参数。

-------
例子
-------

.. code-block:: javascript

    myContract.events.MyEvent({
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // 使用数组表示 或：如 20 或 23。
        fromBlock: 0
    }, function(error, event){ console.log(event); })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on('data', function(event){
        console.log(event); // 与上述可选的回调结果相同
    })
    .on('changed', function(event){
        // 从本地数据库中删除事件
    })
    .on('error', function(error, receipt) { // 如果交易被网络拒绝并带有交易收据，第二个参数将是交易收据。
        ...
    });

    // 事件输出例子
    > {
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }


------------------------------------------------------------------------------

events.allEvents
=====================

.. code-block:: javascript

    myContract.events.allEvents([options][, callback])

和 :ref:`事件 <contract-events>` 相同，只是会接收合约的所有事件。
过滤属性可以选择性地过滤这些事件。


------------------------------------------------------------------------------


getPastEvents
=====================

.. code-block:: javascript

    myContract.getPastEvents(event[, options][, callback])

读取合约历史事件。

----------
参数
----------

1. ``event`` - ``String``: 合约事件名，或者用 ``"allEvents"`` 读取所有事件。
2. ``options`` - ``Object`` （可选）: 用于部署的选项。
    * ``filter`` - ``Object`` （可选）: 按索引参数过滤事件, 例如 ``{filter: {myNumber: [12,13]}}`` 表示 "myNumber" 为 12 或 13 的所有事件。
    * ``fromBlock`` - ``Number|String|BN|BigNumber`` （可选）: 读取从该编号开始的区块中的历史事件（大于或等于该区块号）。 也可以使用预先定义的区块编号，比如 ``"latest"``, ``"earliest"``, ``"pending"``, ``"genesis"`` 。
    * ``toBlock`` - ``Number|String|BN|BigNumber`` （可选）: 读取截止到该编号的区块中的历史事件（小于或等于该区块号）（默认值为 "latest"）。 也可以使用预先定义的区块编号，比如 ``"latest"``, ``"earliest"``, ``"pending"``, ``"genesis"`` 。
    * ``topics`` - ``Array`` （可选）: 用来手动设置事件过滤器的主题。如果设置了 filter 属性和事件签名，则不会自动设置（topic [0]）。
3. ``callback`` - ``Function`` （可选）: 回调函数，触发时其第一个参数为错误对象，第二个参数为历史事件日志数组。


----------
返回值
----------

``Promise`` 返回 ``Array``: 满足给定事件或过滤条件的历史事件对象数组。

对于返回的事件对象结构，参见 :ref:`getPastEvents 返回值 <contract-events-return>`。

-------
例子
-------

.. code-block:: javascript

    myContract.getPastEvents('MyEvent', {
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // 使用数组表示 或：如 20 或 23。
        fromBlock: 0,
        toBlock: 'latest'
    }, function(error, events){ console.log(events); })
    .then(function(events){
        console.log(events) // 与上述可选回调结果相同
    });

    > [{
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    },{
        ...
    }]

