.. _eth-personal:

==================
web3.eth.personal
==================


``web3-eth-personal`` 包让你可以同以太坊节点上的账户进行交互。

.. note:: 这个包中的许多函数都是要发送像密码这种敏感信息的，因此不要在不安全的 Websocket 或 HTTP 服务提供器上调用这些函数，因为你的密码是明文发送的！


.. code-block:: javascript

    var Personal = require('web3-eth-personal');

    // "Personal.providers.givenProvider" 在支持以太坊的浏览器上会被自动设置。
    var personal = new Personal(Personal.givenProvider || 'ws://some.local-or-remote.node:8546');


    // 或者使用 web3 包

    var Web3 = require('web3');
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    // -> web3.eth.personal


------------------------------------------------------------------------------


.. include:: include_package-core.rst



------------------------------------------------------------------------------



newAccount
===========

.. code-block:: javascript

    web3.eth.personal.newAccount(password, [callback])

创建一个新的账户。

.. note:: 永远不要通过不安全的 Websocket 或 HTTP 服务提供器来调用这些函数，因为你的密码是明文发送的！

----------
参数
----------

1. ``password`` - ``String``: 用来加密账户的密码。

-------
返回值
-------

``Promise<string>`` ：新创建账户地址

-------
例子
-------

.. code-block:: javascript

    web3.eth.personal.newAccount('!@superpassword')
    .then(console.log);
    > '0x1234567891011121314151617181920212223456'

------------------------------------------------------------------------------


sign
=====================

.. code-block:: javascript

    web3.eth.personal.sign(dataToSign, address, password [, callback])

该方法通过下面的方式计算一个以太坊特定签名：

.. code-block:: javascript

    sign(keccak256("\x19Ethereum Signed Message:\n" + dataToSign.length + dataToSign)))

在消息前加个前缀使得算出的签名可以被识别为以太坊特定签名。

如果你同时有原始消息和签名消息，就可以使用 :ref:`web3.eth.personal.ecRecover <eth-personal-ecRecover>` (下面有示例) 来恢复签名账户地址


.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``String`` - 要签名的数据。 如果是字符串会使用 :ref:`web3.utils.utf8ToHex <utils-utf8tohex>` 将其转换为 16 进制。
2. ``String`` -  用来签名的账户地址。
3. ``String`` - 用来签名的账户密码。
4. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为签名结果。

-------
返回值
-------


``Promise<string>`` - 签名字符串。

-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.sign("Hello world", "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe", "test password!")
    .then(console.log);
    > "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"

    // 下面代码实现同样功能
    web3.eth.personal.sign(web3.utils.utf8ToHex("Hello world"), "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe", "test password!")
    .then(console.log);
    > "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"

    // 使用原始消息和签名消息来恢复签名账户地址
    web3.eth.personal.ecRecover("Hello world", "0x30755ed65396...etc...")
    .then(console.log);
    > "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe"


------------------------------------------------------------------------------


ecRecover
=====================

.. code-block:: javascript

    web3.eth.personal.ecRecover(dataThatWasSigned, signature [, callback])

恢复数据签名帐户。

----------
参数
----------


1. ``String`` - 被签名的数据。 如果是字符串会使用 :ref:`web3.utils.utf8ToHex <utils-utf8tohex>` 将其转换为 16 进制。
2. ``String`` - 签名。
3. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为签名结果。

-------
返回值
-------


``Promise<string>`` - 签名账户。

-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.ecRecover("Hello world", "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400").then(console.log);
    > "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe"

------------------------------------------------------------------------------

signTransaction
=====================

.. code-block:: javascript

    web3.eth.personal.signTransaction(transaction, password [, callback])

对交易进行签名，账户必须先解锁。

.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``Object`` - 要签名的交易数据，更多详情请看 :ref:`web3.eth.sendTransaction() <eth-sendtransaction>` 。
2. ``String`` - 用来签名交易的 ``from`` 账户密码。
3. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为签名结果。


-------
返回值
-------


``Promise<Object>`` - RLP 编码的交易对象，其 ``raw`` 属性可以用来通过 :ref:`web3.eth.sendSignedTransaction <eth-sendsignedtransaction>` 来发送交易。

-------
例子
-------


.. code-block:: javascript

    web3.eth.signTransaction({
        from: "0xEB014f8c8B418Db6b45774c326A0E64C78914dC0",
        gasPrice: "20000000000",
        gas: "21000",
        to: '0x3535353535353535353535353535353535353535',
        value: "1000000000000000000",
        data: ""
    }, 'MyPassword!').then(console.log);
    > {
        raw: '0xf86c808504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a04f4c17305743700648bc4f6cd3038ec6f6af0df73e31757007b7f59df7bee88da07e1941b264348e80c78c4027afc65a87b0a5e43e86742b8ca0823584c6788fd0',
        tx: {
            nonce: '0x0',
            gasPrice: '0x4a817c800',
            gas: '0x5208',
            to: '0x3535353535353535353535353535353535353535',
            value: '0xde0b6b3a7640000',
            input: '0x',
            v: '0x25',
            r: '0x4f4c17305743700648bc4f6cd3038ec6f6af0df73e31757007b7f59df7bee88d',
            s: '0x7e1941b264348e80c78c4027afc65a87b0a5e43e86742b8ca0823584c6788fd0',
            hash: '0xda3be87732110de6c1354c83770aae630ede9ac308d9f7b399ecfba23d923384'
        }
    }

------------------------------------------------------------------------------

sendTransaction
=====================

.. code-block:: javascript

    web3.eth.personal.sendTransaction(transactionOptions, password [, callback])

该方法用来通过账户管理 API 来发送交易。

.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``Object`` - 交易对象属性
2. ``String`` - 当前帐户的密码
3. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为签名结果。


-------
返回值
-------


``Promise<string>`` - 交易哈希

-------
例子
-------


.. code-block:: javascript

    web3.eth.sendTransaction({
        from: "0xEB014f8c8B418Db6b45774c326A0E64C78914dC0",
        gasPrice: "20000000000",
        gas: "21000",
        to: '0x3535353535353535353535353535353535353535',
        value: "1000000000000000000",
        data: ""
    }, 'MyPassword!').then(console.log);
    > '0xda3be87732110de6c1354c83770aae630ede9ac308d9f7b399ecfba23d923384'

------------------------------------------------------------------------------


unlockAccount
=====================

.. code-block:: javascript

    web3.eth.personal.unlockAccount(address, password, unlockDuraction [, callback])

解锁账户

.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``address`` - ``String``: 要解锁的账户地址。
2. ``password`` - ``String`` - 账户密码。
3. ``unlockDuration`` - ``Number`` - 将帐户保持在解锁状态的持续时间。

-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.unlockAccount("0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe", "test password!", 600)
    .then(console.log('Account unlocked!'));
    > "Account unlocked!"

------------------------------------------------------------------------------

lockAccount
=====================

.. code-block:: javascript

    web3.eth.personal.lockAccount(address [, callback])

锁定给定帐户。

.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``address`` - ``String``: 要锁定的账户地址。
4. ``Function`` - (可选) 可选的回调函数，其第一个参数为错误对象，第二个参数为签名结果。


-------
返回值
-------


``Promise<boolean>``


-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.lockAccount("0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe")
    .then(console.log('Account locked!'));
    > "Account locked!"

------------------------------------------------------------------------------

.. _personal-getaccounts:

getAccounts
=====================

.. code-block:: javascript

    web3.eth.personal.getAccounts([callback])

通过使用服务提供器并调用 RPC 方法 ``personal_listAccounts`` 返回节点控制的账户列表。使用 :ref:`web3.eth.accounts.create() <accounts-create>`
创建的账户不会被添加到这个列表中。这方面的更多信息可以查看 :ref:`web3.eth.personal.newAccount() <personal-newaccount>`。

结果和 :ref:`web3.eth.getAccounts() <eth-getaccounts>` 是一样的，只是它用的 RPC 方法是 ``eth_accounts``。

-------
返回值
-------


``Promise<Array>`` - 节点控制地址数组

-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.getAccounts()
    .then(console.log);
    > ["0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe", "0xDCc6960376d6C6dEa93647383FfB245CfCed97Cf"]

------------------------------------------------------------------------------

importRawKey
=====================

.. code-block:: javascript

    web3.eth.personal.importRawKey(privateKey, password)

将给定的私钥导入密钥存储区，并使用密码对其进行加密。

返回和导入私钥对应的新账户地址。

.. note:: 通过不安全的 HTTP RPC 连接发送帐户密码非常危险。

----------
参数
----------


1. ``privateKey`` - ``String`` - 为加密的私钥 (16 进制字符串)。
2. ``password`` - ``String`` - 账户密码。


-------
返回值
-------


``Promise<string>`` - 账户地址。

-------
例子
-------


.. code-block:: javascript

    web3.eth.personal.importRawKey("cd3376bb711cb332ee3fb2ca04c6a8b9f70c316fcdf7a1f44ef4c7999483295e", "password1234")
    .then(console.log);
    > "0x8f337bf484b2fc75e4b0436645dcc226ee2ac531"


