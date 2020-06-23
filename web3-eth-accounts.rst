.. _eth-accounts:

==================
web3.eth.accounts
==================

``web3.eth.accounts`` 包中包含用于生成以太坊账户和用来签名交易与数据的一系列函数。

.. note:: 该程序包尚未经过安全评审，可能存在安全隐患。在用于生产环境之前，请采取必要的措施来合理的清理内存，安全的存储私钥并完整测试交易的接收和发送功能！

可参照以下代码来独立使用这个包：

.. code-block:: javascript

    var Accounts = require('web3-eth-accounts');

    // 必须传入 eth 或 web3 包才能自动获取调用 account.signTransaction() 所需要的 chainId，gasPrice 和 nonce。
    var accounts = new Accounts('ws://localhost:8546');



------------------------------------------------------------------------------

.. _accounts-create:

create
=====================

.. code-block:: javascript

    web3.eth.accounts.create([entropy]);

生成具有公私钥的账户对象。

----------
参数
----------

1. ``entropy`` - ``String`` (可选): 用于增加混淆度的随机字符串，至少 32 字符长。如果未设定将使用 :ref:`randomhex <randomhex>` 生成一个随机字符串。

.. _eth-accounts-create-return:

-------
返回值
-------

``Object`` - 账户对象，具有如下结构：

    - ``address`` - ``string``: 账户地址。
    - ``privateKey`` - ``string``: 帐户私钥。绝对不要将其共享或以未加密的形式存储在 localstorage 中，并确保使用后将内存 ``清空`` 。
    - ``signTransaction(tx [, callback])`` - ``Function``: 用来签名交易的函数. 更多信息可参考 :ref:`web3.eth.accounts.signTransaction() <eth-accounts-signtransaction>` 。
    - ``sign(data)`` - ``Function``: 用来签名数据的函数. 更多信息可参考 :ref:`web3.eth.accounts.sign() <eth-accounts-sign>` 。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.create();
    > {
        address: "0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01",
        privateKey: "0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709",
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }

    web3.eth.accounts.create('2435@#@#@±±±±!!!!678543213456764321§34567543213456785432134567');
    > {
        address: "0xF2CD2AA0c7926743B1D4310b2BC984a0a453c3d4",
        privateKey: "0xd7325de5c2c1cf0009fac77d3d04a9c004b038883446b065871bc3e831dcd098",
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }

    web3.eth.accounts.create(web3.utils.randomHex(32));
    > {
        address: "0xe78150FaCD36E8EB00291e251424a0515AA1FF05",
        privateKey: "0xcc505ee6067fba3f6fc2050643379e190e087aeffe5d958ab9f2f3ed3800fa4e",
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }

------------------------------------------------------------------------------


privateKeyToAccount
=====================

.. code-block:: javascript

    web3.eth.accounts.privateKeyToAccount(privateKey);

通过私钥来创建账户对象。

----------
参数
----------

1. ``privateKey`` - ``String``: 用来创建账户的私钥。

-------
返回值
-------

``Object`` - 账户对象， :ref:`数据结构见这里 <eth-accounts-create-return>`.

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.privateKeyToAccount('0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709');
    > {
        address: '0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01',
        privateKey: '0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709',
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }

------------------------------------------------------------------------------

.. _eth-accounts-signtransaction:

signTransaction
=====================

.. code-block:: javascript

    web3.eth.accounts.signTransaction(tx, privateKey [, callback]);

使用给定的私钥签名以太坊交易。

----------
参数
----------

1. ``tx`` - ``Object``: 交易对象，具有如下结构：
    - ``nonce`` - ``String``: (可选) 签名交易时所用的 nonce 值。默认使用 :ref:`web3.eth.getTransactionCount() <eth-gettransactioncount>`.
    - ``chainId`` - ``String``: (可选) 签名交易时所用的 chain id。 默认使用 :ref:`web3.eth.net.getId() <net-getid>`.
    - ``to`` - ``String``: (可选) 交易接受者，部署合约时其值为空。
    - ``data`` - ``String``: (可选) 交易调用数据，对简单转账交易来说其值为空。
    - ``value`` - ``String``: (可选) 以 wei 为单位的以太币转账数量。
    - ``gasPrice`` - ``String``: (可选) 交易所用的燃料价格，如果传入为空值则会使用 :ref:`web3.eth.gasPrice() <eth-gasprice>`
    - ``gas`` - ``String``: 交易可用的燃料上限。
    - ``chain`` - ``String``: (可选) 默认值为 ``mainnet``.
    - ``hardfork`` - ``String``: (可选) 默认值为 ``petersburg``.
    - ``common`` - ``Object``: (可选) common 对象。
        - ``customChain`` - ``Object``: 自定义链属性
            - ``name`` - ``string``: (可选) 链名称
            - ``networkId`` - ``number``: 自定义链的网络 id
            - ``chainId`` - ``number``: 自定义链的 chain id
        - ``baseChain`` - ``string``: (可选) ``mainnet``, ``goerli``, ``kovan``, ``rinkeby``, 或 ``ropsten``
        - ``hardfork`` - ``string``: (可选) ``chainstart``, ``homestead``, ``dao``, ``tangerineWhistle``, ``spuriousDragon``, ``byzantium``, ``constantinople``, ``petersburg``, 或 ``istanbul``
2. ``privateKey`` - ``String``: 签名交易所用的私钥。
3. ``callback`` - ``Function``: (可选) 可选的回调函数, 其第一个返回值为错误对象，第二个返回值为调用结果。


-------
返回值
-------

``Promise`` 返回 ``Object``: RLP 编码的交易对象：
    - ``messageHash`` - ``String``: 给定消息的哈希值。
    - ``r`` - ``String``: 签名的头 32 个字节。
    - ``s`` - ``String``: 签名接下来的 32 个字节。
    - ``v`` - ``String``: 恢复值 + 27。
    - ``rawTransaction`` - ``String``: RLP 编码的交易, 可使用 :ref:`web3.eth.sendSignedTransaction <eth-sendsignedtransaction>` 直接发送。
    - ``transactionHash`` - ``String``: RLP 编码交易的交易哈希。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.signTransaction({
        to: '0xF0109fC8DF283027b6285cc889F5aA624EaC1F55',
        value: '1000000000',
        gas: 2000000
    }, '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318')
    .then(console.log);
    > {
        messageHash: '0x31c2f03766b36f0346a850e78d4f7db2d9f4d7d54d5f272a750ba44271e370b1',
        v: '0x25',
        r: '0xc9cf86333bcb065d140032ecaab5d9281bde80f21b9687b3e94161de42d51895',
        s: '0x727a108a0b8d101465414033c3f705a9c7b826e596766046ee1183dbc8aeaa68',
        rawTransaction: '0xf869808504e3b29200831e848094f0109fc8df283027b6285cc889f5aa624eac1f55843b9aca008025a0c9cf86333bcb065d140032ecaab5d9281bde80f21b9687b3e94161de42d51895a0727a108a0b8d101465414033c3f705a9c7b826e596766046ee1183dbc8aeaa68'
        transactionHash: '0xde8db924885b0803d2edc335f745b2b8750c8848744905684c20b987443a9593'
    }

    web3.eth.accounts.signTransaction({
        to: '0xF0109fC8DF283027b6285cc889F5aA624EaC1F55',
        value: '1000000000',
        gas: 2000000,
        gasPrice: '234567897654321',
        nonce: 0,
        chainId: 1
    }, '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318')
    .then(console.log);
    > {
        messageHash: '0x6893a6ee8df79b0f5d64a180cd1ef35d030f3e296a5361cf04d02ce720d32ec5',
        r: '0x9ebb6ca057a0535d6186462bc0b465b561c94a295bdb0621fc19208ab149a9c',
        s: '0x440ffd775ce91a833ab410777204d5341a6f9fa91216a6f3ee2c051fea6a0428',
        v: '0x25',
        rawTransaction: '0xf86a8086d55698372431831e848094f0109fc8df283027b6285cc889f5aa624eac1f55843b9aca008025a009ebb6ca057a0535d6186462bc0b465b561c94a295bdb0621fc19208ab149a9ca0440ffd775ce91a833ab410777204d5341a6f9fa91216a6f3ee2c051fea6a0428'
        transactionHash: '0xd8f64a42b57be0d565f385378db2f6bf324ce14a594afc05de90436e9ce01f60'
    }

    // 或者带有 common 对象
    web3.eth.accounts.signTransaction({
        to: '0xF0109fC8DF283027b6285cc889F5aA624EaC1F55',
        value: '1000000000',
        gas: 2000000
        common: {
          baseChain: 'mainnet',
          hardfork: 'petersburg',
          customChain: {
            name: 'custom-chain',
            chainId: 1,
            networkId: 1
          }
        }
    }, '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318')
    .then(console.log);




------------------------------------------------------------------------------


recoverTransaction
=====================

.. code-block:: javascript

    web3.eth.accounts.recoverTransaction(rawTransaction);

恢复用于签名给定 RLP 编码交易的以太坊地址。

----------
参数
----------

1. ``signature`` - ``String``: RLP 编码的交易。


-------
返回值
-------

``String``: 用于签名该交易的以太坊地址。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.recoverTransaction('0xf86180808401ef364594f0109fc8df283027b6285cc889f5aa624eac1f5580801ca031573280d608f75137e33fc14655f097867d691d5c4c44ebe5ae186070ac3d5ea0524410802cdc025034daefcdfa08e7d2ee3f0b9d9ae184b2001fe0aff07603d9');
    > "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55"



------------------------------------------------------------------------------

hashMessage
=====================

.. code-block:: javascript

    web3.eth.accounts.hashMessage(message);

计算给定消息的哈希值，以便于用于 :ref:`web3.eth.accounts.recover() <accounts-recover>` 函数。 数据采用 UTF-8 十六进制解码封装: ``"\x19Ethereum Signed Message:\n" + message.length + message``，并使用 keccak256 进行哈希运算。

----------
参数
----------

1. ``message`` - ``String``: 要进行哈希运算的消息，如果是 16 进制字符串，首先对其进行 UTF-8 解码。


-------
返回值
-------

``String``: 哈希运算后的消息

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.hashMessage("Hello World")
    > "0xa1de988600a42c4b4ab089b619297c17d53cffae5d5120d82d8a92d0bb3b78f2"

    // 下面这种方式可以得到同样的哈希值
    web3.eth.accounts.hashMessage(web3.utils.utf8ToHex("Hello World"))
    > "0xa1de988600a42c4b4ab089b619297c17d53cffae5d5120d82d8a92d0bb3b78f2"



------------------------------------------------------------------------------

.. _eth-accounts-sign:

sign
=====================

.. code-block:: javascript

    web3.eth.accounts.sign(data, privateKey);

签名任意数据。

----------
参数
----------

1. ``data`` - ``String``: 要签名的数据。
2. ``privateKey`` - ``String``: 用以签名的私钥。

.. note:: 传入的 `data` 参数将用这种方式进行 UTF-8 十六进制解码封装: ``"\x19Ethereum Signed Message:\n" + message.length + message``.

-------
返回值
-------

``Object``: 签名对象
    - ``message`` - ``String``: 给定消息。
    - ``messageHash`` - ``String``: 给定消息的哈希值。
    - ``r`` - ``String``: 签名的头 32 个字节。
    - ``s`` - ``String``: 签名接下来的 32 个字节。
    - ``v`` - ``String``: 恢复值(Recovery) + 27。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.sign('Some data', '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318');
    > {
        message: 'Some data',
        messageHash: '0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655',
        v: '0x1c',
        r: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd',
        s: '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029',
        signature: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c'
    }



------------------------------------------------------------------------------

.. _accounts-recover:

recover
=====================

.. code-block:: javascript

    web3.eth.accounts.recover(signatureObject);
    web3.eth.accounts.recover(message, signature [, preFixed]);
    web3.eth.accounts.recover(message, v, r, s [, preFixed]);

恢复用于签名给定数据的以太坊地址。

----------
参数
----------

1. ``message|signatureObject`` - ``String|Object``: 要么是签名消息或哈希，要么是带有下面属性的签名对象：
    - ``messageHash`` - ``String``: 给定消息的哈希值，该消息已加前缀 ``"\x19Ethereum Signed Message:\n" + message.length + message``。
    - ``r`` - ``String``: 签名的头 32 个字节。
    - ``s`` - ``String``: 签名接下来的 32 个字节。
    - ``v`` - ``String``: 恢复值 + 27。
2. ``signature`` - ``String``: 原始 RLP 编码签名或者通过第 2-4 号参数指定 v，r，s 值。
3. ``preFixed`` - ``Boolean`` (可选, 默认值: ``false``): 如果其值为 ``true``, 给定的消息不会自动添加前缀 ``"\x19Ethereum Signed Message:\n" + message.length + message``, 假定给定消息已经带了该前缀。


-------
返回值
-------

``String``: 用于签名此数据的以太坊地址。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.recover({
        messageHash: '0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655',
        v: '0x1c',
        r: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd',
        s: '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029'
    })
    > "0x2c7536E3605D9C16a7a3D7b1898e529396a65c23"

    // 签名消息, 签名
    web3.eth.accounts.recover('Some data', '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c');
    > "0x2c7536E3605D9C16a7a3D7b1898e529396a65c23"

    // 签名消息, v, r, s
    web3.eth.accounts.recover('Some data', '0x1c', '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd', '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029');
    > "0x2c7536E3605D9C16a7a3D7b1898e529396a65c23"



------------------------------------------------------------------------------


encrypt
=====================

.. code-block:: javascript

    web3.eth.accounts.encrypt(privateKey, password);

将私钥加密变换为 keystore v3 标准格式。

----------
参数
----------

1. ``privateKey`` - ``String``: 要加密的私钥。
2. ``password`` - ``String``: 用于加密的密码。


-------
返回值
-------

``Object``: 加密后的 keystore v3 JSON。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.encrypt('0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318', 'test!')
    > {
        version: 3,
        id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',
        address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',
        crypto: {
            ciphertext: 'a1c25da3ecde4e6a24f3697251dd15d6208520efc84ad97397e906e6df24d251',
            cipherparams: { iv: '2885df2b63f7ef247d753c82fa20038a' },
            cipher: 'aes-128-ctr',
            kdf: 'scrypt',
            kdfparams: {
                dklen: 32,
                salt: '4531b3c174cc3ff32a6a7a85d6761b410db674807b2d216d022318ceee50be10',
                n: 262144,
                r: 8,
                p: 1
            },
            mac: 'b8b010fff37f9ae5559a352a185e86f9b9c1d7f7a9f1bd4e82a5dd35468fc7f6'
        }
    }



------------------------------------------------------------------------------

decrypt
=====================

.. code-block:: javascript

    web3.eth.accounts.decrypt(keystoreJsonV3, password);

解密 keystore v3 JSON，并创建账户。

----------
参数
----------

1. ``encryptedPrivateKey`` - ``String``: 要进行解密的加密私钥。
2. ``password`` - ``String``: 用于加密的密码。


-------
返回值
-------

``Object``: 解密的帐户对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.decrypt({
        version: 3,
        id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',
        address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',
        crypto: {
            ciphertext: 'a1c25da3ecde4e6a24f3697251dd15d6208520efc84ad97397e906e6df24d251',
            cipherparams: { iv: '2885df2b63f7ef247d753c82fa20038a' },
            cipher: 'aes-128-ctr',
            kdf: 'scrypt',
            kdfparams: {
                dklen: 32,
                salt: '4531b3c174cc3ff32a6a7a85d6761b410db674807b2d216d022318ceee50be10',
                n: 262144,
                r: 8,
                p: 1
            },
            mac: 'b8b010fff37f9ae5559a352a185e86f9b9c1d7f7a9f1bd4e82a5dd35468fc7f6'
        }
    }, 'test!');
    > {
        address: "0x2c7536E3605D9C16a7a3D7b1898e529396a65c23",
        privateKey: "0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318",
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }



------------------------------------------------------------------------------
.. _eth_accounts_wallet:

wallet
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet;

一个多账户内存钱包，这些账户可以用于 :ref:`web3.eth.sendTransaction() <eth-sendtransaction>`。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet;
    > Wallet {
        0: {...}, // 账户索引
        "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55": {...},  // 账户地址
        "0xf0109fc8df283027b6285cc889f5aa624eac1f55": {...},  // 全小写账户地址
        1: {...},
        "0xD0122fC8DF283027b6285cc889F5aA624EaC1d23": {...},
        "0xd0122fc8df283027b6285cc889f5aa624eac1d23": {...},

        add: function(){},
        remove: function(){},
        save: function(){},
        load: function(){},
        clear: function(){},

        length: 2,
    }



------------------------------------------------------------------------------

wallet.create
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.create(numberOfAccounts [, entropy]);

在钱包中创建一个或多个账户。不会覆盖已经存在的钱包。

----------
参数
----------

1. ``numberOfAccounts`` - ``Number``: 要创建的账户数量。设为空值时创建空钱包。
2. ``entropy`` - ``String`` (可选): 创建账户时为增加随机性而使用的随机字符串，至少 32 个字符长。


-------
返回值
-------

``Object``: 钱包对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.create(2, '54674321§3456764321§345674321§3453647544±±±§±±±!!!43534534534534');
    > Wallet {
        0: {...},
        "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55": {...},
        "0xf0109fc8df283027b6285cc889f5aa624eac1f55": {...},
        ...
    }



------------------------------------------------------------------------------

wallet.add
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.add(account);

使用私钥或账户对象向钱包中添加账户。

----------
参数
----------

1. ``account`` - ``String|Object``: 私钥或者通过 :ref:`web3.eth.accounts.create() <accounts-create>` 创建的账户对象。


-------
返回值
-------

``Object``: 所添加的账户。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.add('0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318');
    > {
        index: 0,
        address: '0x2c7536E3605D9C16a7a3D7b1898e529396a65c23',
        privateKey: '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318',
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }

    web3.eth.accounts.wallet.add({
        privateKey: '0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709',
        address: '0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01'
    });
    > {
        index: 0,
        address: '0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01',
        privateKey: '0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709',
        signTransaction: function(tx){...},
        sign: function(data){...},
        encrypt: function(password){...}
    }



------------------------------------------------------------------------------

wallet.remove
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.remove(account);

从钱包中移除账户。

----------
参数
----------

1. ``account`` - ``String|Number``: 账户地址，或者账户在钱包中的索引。


-------
返回值
-------

``Boolean``: 移除成功则返回 ``true`` ，否则返回 ``false`` 。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet;
    > Wallet {
        0: {...},
        "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55": {...}
        1: {...},
        "0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01": {...}
        ...
    }

    web3.eth.accounts.wallet.remove('0xF0109fC8DF283027b6285cc889F5aA624EaC1F55');
    > true

    web3.eth.accounts.wallet.remove(3);
    > false



------------------------------------------------------------------------------


wallet.clear
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.clear();

安全地清空钱包并移除全部账户。

----------
参数
----------

无

-------
返回值
-------

``Object``: 钱包对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.clear();
    > Wallet {
        add: function(){},
        remove: function(){},
        save: function(){},
        load: function(){},
        clear: function(){},

        length: 0
    }


------------------------------------------------------------------------------

wallet.encrypt
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.encrypt(password);

加密所有的钱包账户为 keystore v3 对象。

----------
参数
----------

1. ``password`` - ``String``: 用于加密的密钥。


-------
返回值
-------

``Array``: 已加密的 keystore v3 对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.encrypt('test');
    > [ { version: 3,
        id: 'dcf8ab05-a314-4e37-b972-bf9b86f91372',
        address: '06f702337909c06c82b09b7a22f0a2f0855d1f68',
        crypto:
         { ciphertext: '0de804dc63940820f6b3334e5a4bfc8214e27fb30bb7e9b7b74b25cd7eb5c604',
           cipherparams: [Object],
           cipher: 'aes-128-ctr',
           kdf: 'scrypt',
           kdfparams: [Object],
           mac: 'b2aac1485bd6ee1928665642bf8eae9ddfbc039c3a673658933d320bac6952e3' } },
      { version: 3,
        id: '9e1c7d24-b919-4428-b10e-0f3ef79f7cf0',
        address: 'b5d89661b59a9af0b34f58d19138baa2de48baaf',
        crypto:
         { ciphertext: 'd705ebed2a136d9e4db7e5ae70ed1f69d6a57370d5fbe06281eb07615f404410',
           cipherparams: [Object],
           cipher: 'aes-128-ctr',
           kdf: 'scrypt',
           kdfparams: [Object],
           mac: 'af9eca5eb01b0f70e909f824f0e7cdb90c350a802f04a9f6afe056602b92272b' } }
    ]

------------------------------------------------------------------------------


wallet.decrypt
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.decrypt(keystoreArray, password);

解密 keystore v3 对象。

----------
参数
----------

1. ``keystoreArray`` - ``Array``: 要解密的加密 keystore v3 对象。
2. ``password`` - ``String``: 用来加密的密钥。


-------
返回值
-------

``Object``: 钱包对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.decrypt([
      { version: 3,
      id: '83191a81-aaca-451f-b63d-0c5f3b849289',
      address: '06f702337909c06c82b09b7a22f0a2f0855d1f68',
      crypto:
       { ciphertext: '7d34deae112841fba86e3e6cf08f5398dda323a8e4d29332621534e2c4069e8d',
         cipherparams: { iv: '497f4d26997a84d570778eae874b2333' },
         cipher: 'aes-128-ctr',
         kdf: 'scrypt',
         kdfparams:
          { dklen: 32,
            salt: '208dd732a27aa4803bb760228dff18515d5313fd085bbce60594a3919ae2d88d',
            n: 262144,
            r: 8,
            p: 1 },
         mac: '0062a853de302513c57bfe3108ab493733034bf3cb313326f42cf26ea2619cf9' } },
       { version: 3,
      id: '7d6b91fa-3611-407b-b16b-396efb28f97e',
      address: 'b5d89661b59a9af0b34f58d19138baa2de48baaf',
      crypto:
       { ciphertext: 'cb9712d1982ff89f571fa5dbef447f14b7e5f142232bd2a913aac833730eeb43',
         cipherparams: { iv: '8cccb91cb84e435437f7282ec2ffd2db' },
         cipher: 'aes-128-ctr',
         kdf: 'scrypt',
         kdfparams:
          { dklen: 32,
            salt: '08ba6736363c5586434cd5b895e6fe41ea7db4785bd9b901dedce77a1514e8b8',
            n: 262144,
            r: 8,
            p: 1 },
         mac: 'd2eb068b37e2df55f56fa97a2bf4f55e072bef0dd703bfd917717d9dc54510f0' } }
    ], 'test');
    > Wallet {
        0: {...},
        1: {...},
        "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55": {...},
        "0xD0122fC8DF283027b6285cc889F5aA624EaC1d23": {...}
        ...
    }



------------------------------------------------------------------------------

wallet.save
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.save(password [, keyName]);

将钱包加密并在 local storage 中保存为字符串。

.. note::  仅在浏览器中可用。

----------
参数
----------

1. ``password`` - ``String``: 用来加密钱包的密钥。
2. ``keyName`` - ``String``: (可选) 用来在 local storage 中寻址的健值, 默认值为 ``"web3js_wallet"``。


-------
返回值
-------

``Boolean``

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.save('test#!$');
    > true


------------------------------------------------------------------------------

wallet.load
=====================

.. code-block:: javascript

    web3.eth.accounts.wallet.load(password [, keyName]);

从 local storage 中载入钱包并解密。

.. note::  仅限于浏览器使用。

----------
参数
----------

1. ``password`` - ``String``: 用来解密钱包的密钥。
2. ``keyName`` - ``String``: (可选) 用来在 local storage 中寻址的健值, 默认值为 ``"web3js_wallet"``。


-------
返回值
-------

``Object``: 钱包对象。

-------
示例代码
-------

.. code-block:: javascript

    web3.eth.accounts.wallet.load('test#!$', 'myWalletKey');
    > Wallet {
        0: {...},
        1: {...},
        "0xF0109fC8DF283027b6285cc889F5aA624EaC1F55": {...},
        "0xD0122fC8DF283027b6285cc889F5aA624EaC1d23": {...}
        ...
    }
