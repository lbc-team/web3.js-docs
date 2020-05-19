.. _eth-ens:

=============
web3.eth.ens
=============

``web3.eth.ens`` 相关函数让你可以与 ENS 进行交互。
------------------------------------------------------------------------------

registry
=====================

.. code-block:: javascript

    web3.eth.ens.registry;

返回指定网络的 ENS 注册表。

-------
返回值
-------

``Registry`` - 当前 ENS 注册表。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.registry;
    > {
        ens: ENS,
        contract: Contract,
        owner: Function(name),
        resolve: Function(name)
    }

------------------------------------------------------------------------------

resolver
=====================

.. code-block:: javascript

    web3.eth.ens.resolver(name);

返回以太坊域名地址对应的 resolver 合约。

-------
返回值
-------

``Reslver`` - 该域名对应的 ENS resolver。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.resolver('ethereum.eth').then(function (contract) {
        console.log(contract);
    });
    > Contract<Resolver>

------------------------------------------------------------------------------

getAddress
=====================

.. code-block:: javascript

    web3.eth.ens.getAddress(ENSName);

将 ENS 域名解析为以太坊地址。

----------
参数
----------

1. ``ENSName`` - ``String``: 需哟解析的 ENS 域名。

-------
返回值
-------

``String`` - 给定域名的以太坊地址。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.getAddress('ethereum.eth').then(function (address) {
        console.log(address);
    })
    > 0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359

------------------------------------------------------------------------------

setAddress
=====================

.. code-block:: javascript

    web3.eth.ens.setAddress(ENSName, address, options);

通过域名解析器（resolver）指定 ENS 域名对应的以太坊地址。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。
2. ``address`` - ``String``: 要设置的以太坊地址。
3. ``options`` - ``Object``: 用于发送交易的参数选项。
    * ``from`` - ``String``: 交易发出地址。
    * ``gasPrice`` - ``String`` (可选): 用于此交易的燃料价格（以 wei 为单位）。
    * ``gas`` - ``Number`` (可选): 用于此交易的最大燃料数量（gasLimit）。

``AddrChanged`` 事件会被触发。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.setAddress(
        'ethereum.eth',
        '0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    ).then(function (result) {
             console.log(result.events);
    });
    > AddrChanged(...)

    // 或者使用事件触发器

    web3.eth.ens.setAddress(
        'ethereum.eth',
        '0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    )
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('error', console.error);

    // Or listen to the AddrChanged event on the resolver

    web3.eth.ens.resolver('ethereum.eth').then(function (resolver) {
        resolver.events.AddrChanged({fromBlock: 0}, function(error, event) {
            console.log(event);
        })
        .on('data', function(event){
            console.log(event);
        })
        .on('changed', function(event){
            // remove event from local database
        })
        .on('error', console.error);
    });


    关于合约事件处理的更多信息，请看这里 contract-events_。

------------------------------------------------------------------------------

getPubkey
=====================

.. code-block:: javascript

    web3.eth.ens.getPubkey(ENSName);

返回公钥所在曲线点的 X 和 Y 坐标。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。

-------
返回值
-------

``Object<String, String>`` - X 和 Y 坐标。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.getPubkey('ethereum.eth').then(function (result) {
        console.log(result)
    });
    > {
        "0": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "1": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "x": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "y": "0x0000000000000000000000000000000000000000000000000000000000000000"
    }

------------------------------------------------------------------------------

setPubkey
=====================

.. code-block:: javascript

    web3.eth.ens.setPubkey(ENSName, x, y, options);

设置 ENS 节点对应的 SECP256k1 公钥。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。
2. ``x`` - ``String``: 公钥的 X 坐标。
3. ``y`` - ``String``: 公钥的 Y 坐标。
4. ``options`` - ``Object``: 用于发送交易的参数选型。
    * ``from`` - ``String``: 交易发出地址。
    * ``gasPrice`` - ``String`` (可选): 用于此交易的燃料价格（以 wei 为单位）。
    * ``gas`` - ``Number`` (可选): 用于此交易的最大燃料数量（gasLimit）。


触发 ``PubkeyChanged`` 事件。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.setPubkey(
        'ethereum.eth',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    ).then(function (result) {
        console.log(result.events);
    });
    > PubkeyChanged(...)

    // 或者使用事件触发器

    web3.eth.ens.setPubkey(
        'ethereum.eth',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    )
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('error', console.error);

    // 或者监听与 resolver 关联的 PubkeyChanged 事件


    web3.eth.ens.resolver('ethereum.eth').then(function (resolver) {
        resolver.events.PubkeyChanged({fromBlock: 0}, function(error, event) {
            console.log(event);
        })
        .on('data', function(event){
            console.log(event);
        })
        .on('changed', function(event){
            // remove event from local database
        })
        .on('error', console.error);
    });


    关于合约事件处理的更多信息，请看这里 contract-events_。

------------------------------------------------------------------------------

getContent
=====================

.. code-block:: javascript

    web3.eth.ens.getContent(ENSName);

返回与 ENS 节点关联的内容哈希。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。

-------
返回值
-------

``String`` - 与 ENS 节点关联的内容哈希。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.getContent('ethereum.eth').then(function (result) {
        console.log(result);
    });
    > "0x0000000000000000000000000000000000000000000000000000000000000000"

------------------------------------------------------------------------------

setContent
=====================

.. code-block:: javascript

    web3.eth.ens.setContent(ENSName, hash, options);

设置与 ENS 节点关联的内容哈希。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。
2. ``hash`` - ``String``: 要设置的内容哈希。
3. ``options`` - ``Object``: 用于发送交易的参数选型。
    * ``from`` - ``String``: 交易发出地址。
    * ``gasPrice`` - ``String`` (可选): 用于此交易的燃料价格（以 wei 为单位）。
    * ``gas`` - ``Number`` (可选): 用于此交易的最大燃料数量（gasLimit）。


触发 ``ContentChanged`` 事件。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.setContent(
        'ethereum.eth',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    ).then(function (result) {
        console.log(result.events);
    });
    > ContentChanged(...)

    // 或者使用事件触发器

    web3.eth.ens.setContent(
        'ethereum.eth',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    )
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('error', console.error);

    // 或者监听 resolver 上的 ContentChanged 事件

    web3.eth.ens.resolver('ethereum.eth').then(function (resolver) {
        resolver.events.ContentChanged({fromBlock: 0}, function(error, event) {
            console.log(event);
        })
        .on('data', function(event){
            console.log(event);
        })
        .on('changed', function(event){
            // 从本地数据库中移除事件
        })
        .on('error', console.error);
    });


    关于合约事件处理的更多信息，请看这里 contract-events_。

------------------------------------------------------------------------------

getMultihash
=====================

.. code-block:: javascript

    web3.eth.ens.getMultihash(ENSName);

返回和 ENS 节点管理的 multihash。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。

-------
返回值
-------

``String`` - 关联的 multihash。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.getMultihash('ethereum.eth').then(function (result) {
        console.log(result);
    });
    > 'QmXpSwxdmgWaYrgMUzuDWCnjsZo5RxphE3oW7VhTMSCoKK'

------------------------------------------------------------------------------

setMultihash
=====================

.. code-block:: javascript

    web3.eth.ens.setMultihash(ENSName, hash, options);

设置和 ENS 节点关联的 multihash。

----------
参数
----------

1. ``ENSName`` - ``String``: ENS 域名。
2. ``hash`` - ``String``: 要设置的 multihash。
3. ``options`` - ``Object``: 用于发送交易的参数选型。
    * ``from`` - ``String``: 交易发出地址。
    * ``gasPrice`` - ``String`` (可选): 用于此交易的燃料价格（以 wei 为单位）。
    * ``gas`` - ``Number`` (可选): 用于此交易的最大燃料数量（gasLimit）。


触发 ``MultihashChanged`` 事件。

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.setMultihash(
        'ethereum.eth',
        'QmXpSwxdmgWaYrgMUzuDWCnjsZo5RxphE3oW7VhTMSCoKK',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    ).then(function (result) {
        console.log(result.events);
    });
    > MultihashChanged(...)

    // 或者使用事件触发器

    web3.eth.ens.setMultihash(
        'ethereum.eth',
        'QmXpSwxdmgWaYrgMUzuDWCnjsZo5RxphE3oW7VhTMSCoKK',
        {
            from: '0x9CC9a2c777605Af16872E0997b3Aeb91d96D5D8c'
        }
    )
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('error', console.error);


    关于合约事件处理的更多信息，请看这里 contract-events_。

------------------------------------------------------------------------------

ENS events
=====================

ENS 接口提供了监听所有 ENS 相关事件的可能性。

---------------------
已知的 resolver 事件
---------------------

1. AddrChanged(node bytes32, a address)
2. ContentChanged(node bytes32, hash bytes32)
4. NameChanged(node bytes32, name string)
5. ABIChanged(node bytes32, contentType uint256)
6. PubkeyChanged(node bytes32, x bytes32, y bytes32)

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.resolver('ethereum.eth').then(function (resolver) {
        resolver.events.AddrChanged({fromBlock: 0}, function(error, event) {
            console.log(event);
        })
        .on('data', function(event){
            console.log(event);
        })
        .on('changed', function(event){
            // 从本地数据库移除事件
        })
        .on('error', console.error);
    });
    > {
        returnValues: {
            node: '0x123456789...',
            a: '0x123456789...',
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: [
                '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385'
            ]
        },
        event: 'AddrChanged',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }

---------------------
已知的 registry 事件
---------------------

1. Transfer(node bytes32, owner address)
2. NewOwner(node bytes32, label bytes32, owner address)
4. NewResolver(node bytes32, resolver address)
5. NewTTL(node bytes32, ttl uint64)

-------
例子
-------

.. code-block:: javascript

    web3.eth.ens.resistry.then(function (registry) {
        registry.events.Transfer({fromBlock: 0}, , function(error, event) {
              console.log(event);
          })
          .on('data', function(event){
              console.log(event);
          })
          .on('changed', function(event){
              // 从本地数据库移除事件
          })
          .on('error', console.error);
    });
    > {
        returnValues: {
            node: '0x123456789...',
            owner: '0x123456789...',
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: [
                '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385'
            ]
        },
        event: 'Transfer',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }

关于合约事件处理的更多信息，请看这里 contract-events_。

------------------------------------------------------------------------------

