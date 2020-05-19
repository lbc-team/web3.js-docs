.. _utils:

===========
web3.utils
===========

这个包为以太坊 DApp 和其它的 web3.js 包提供了工具性函数。
------------------------------------------------------------------------------

布隆过滤器
=====================

-----------------------
什么是布隆过滤器？
-----------------------

布隆过滤器是种空间利用效率很高的，基于概率的数据结构，用来对集合成员进行快速检测。这么说可能对你来说没什么感觉，那么让我们一起来看看布隆过滤器是如何被使用的。
假定我们有一个很大的数据集合，并且我们想快速检测某个元素当前是否在那个数据集合中。最直接的办法可能就是对集合进行遍历查找，如果数据集合比较小，这么做很可能是可行的。不幸的是，如果我们的数据集合非常大，这种查找办法会耗费相当长的时间。幸运的是，在以太坊中我们用一些巧妙的办法来加快这个查找。
布隆过滤器就是这些巧妙办法之一。它背后的基本思想就是对进入数据集合的每个元素进行哈希运算，从得到的哈希值中提取某些位，然后用这些位去填充一个固定大小的位数组（也就是将数组中对应的位设置为 1）。这个位数组就被称为布隆过滤器。
稍后，当我们想要检测一个元素是否在这个集合中时，我们只需对这个元素进行哈希运算并检查布隆过滤器中对应的位数据。如果哪怕只有一个位对应的数据为 0，则该元素肯定不在这个数据集合中。如果所有位数据都为 1，则该元素有可能在这个数据结合中，我们需要实际查询数据库才能确定。我们可能会遇到假阳性，但肯定不会遇到假阴性，这已经能够大大减少我们查询数据库的次数了。

**实际例子**
在以太坊中，有个布隆过滤器会发挥作用的例子就是接近实时的根据每一个新区块去更新用户余额。如果不在新区块上使用布隆过滤器，即使用户在那个区块上没有任何动作，你也要强制更新用户余额。如果你用了区块中的 logBlooms，你就可以在进行任何更慢的操作之前先对用户地址进行布隆过滤器测试，这将极大地减少你所执行的调用量，因为你只有在确定区块中包含对应用户地址时才会执行那些额外的操作。这会为你的应用带来更高的性能。
---------
相关函数
---------

- `web3.utils.isBloom <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#isbloom>`_
- `web3.utils.isUserEthereumAddressInBloom <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#isuserethereumaddressinbloom>`_
- `web3.utils.isContractAddressInBloom <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#iscontractaddressinbloom>`_
- `web3.utils.isTopic <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#istopic>`_
- `web3.utils.isTopicInBloom <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#istopicinbloom>`_
- `web3.utils.isInBloom <https://github.com/joshstevens19/ethereum-bloom-filters/blob/master/README.md#isinbloom>`_


.. note:: 请在 `这里 <https://github.com/joshstevens19/ethereum-bloom-filters/issues>`_ 提出你遇到的任何问题

------------------------------------------------------------------------------

randomHex
=====================

.. code-block:: javascript

    web3.utils.randomHex(size)

这个 `randomHex <https://github.com/frozeman/randomHex>`_ 库用来根据指定字节大小生成密码学强度的伪随机 16 进制字符串.

----------
参数
----------

1. ``size`` - ``Number``: 16 进制字符串的字节大小, 比如 ``32`` 将生成一个 32 个字节大小的 16 进制字符串，以 "0x" 为前缀的 64 个字符来表示。

-------

返回值
-------

``String``: 生成的 16 进制随机字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.randomHex(32)
    > "0xa5b9d60f32436310afebcfda832817a68921beb782fabf7915cc0460b443116a"

    web3.utils.randomHex(4)
    > "0x6892ffc6"

    web3.utils.randomHex(2)
    > "0x99d6"

    web3.utils.randomHex(1)
    > "0x9a"

    web3.utils.randomHex(0)
    > "0x"




------------------------------------------------------------------------------

_
=====================

.. code-block:: javascript

    web3.utils._()

这个 `underscore <http://underscorejs.org>`_ 库提供了很多方便实用的 JavaScript 函数.

更多详情查看 `underscore API 手册 <http://underscorejs.org>`_ .

-------
例子
-------

.. code-block:: javascript

    var _ = web3.utils._;

    _.union([1,2],[3]);
    > [1,2,3]

    _.each({my: 'object'}, function(value, key){ ... })

    ...



------------------------------------------------------------------------------

.. _utils-bn:

BN
=====================

.. code-block:: javascript

    web3.utils.BN(mixed)

 `BN.js <https://github.com/indutny/bn.js/>`_ 用来在 JavaScript 中进行大数计算。更多详情查看 `BN.js 文档 <https://github.com/indutny/bn.js/>`_

.. note:: 为了在不同类型之间进行安全的类型转换, 包括 `BigNumber.js <http://mikemcl.github.io/bignumber.js/>`_ 可以使用 :ref:`utils.toBN <utils-tobn>`

----------
参数
----------

1. ``mixed`` - ``String|Number``: 要转换为 BN 对象的一个数字字符串或 16 进制字符串.

-------
返回值
-------

``Object``: `BN.js <https://github.com/indutny/bn.js/>`_ 实例.

-------
例子
-------

.. code-block:: javascript

    var BN = web3.utils.BN;

    new BN(1234).toString();
    > "1234"

    new BN('1234').add(new BN('1')).toString();
    > "1235"

    new BN('0xea').toString();
    > "234"


------------------------------------------------------------------------------

isBN
=====================

.. code-block:: javascript

    web3.utils.isBN(bn)


检测给定值是否为 `BN.js <https://github.com/indutny/bn.js/>`_ 实例.


----------
参数
----------

1. ``bn`` - ``Object``: `BN.js <https://github.com/indutny/bn.js/>`_ 实例.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var number = new BN(10);

    web3.utils.isBN(number);
    > true


------------------------------------------------------------------------------

isBigNumber
=====================

.. code-block:: javascript

    web3.utils.isBigNumber(bignumber)


检测给定值是否为 `BigNumber.js <http://mikemcl.github.io/bignumber.js/>`_ 实例.


----------
参数
----------

1. ``bignumber`` - ``Object``: `BigNumber.js <http://mikemcl.github.io/bignumber.js/>`_ 实例.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var number = new BigNumber(10);

    web3.utils.isBigNumber(number);
    > true


------------------------------------------------------------------------------

.. _utils-sha3:

sha3
=====================

.. code-block:: javascript

    web3.utils.sha3(string)
    web3.utils.keccak256(string) // 别名

将计算输入参数的 sha3 值。

.. note::  要模拟 solidity 中使用 sha3，需要使用 :ref:`soliditySha3 <utils-soliditysha3>`

----------
参数
----------

1. ``string`` - ``String``: 要进行哈希运算的字符串.

-------
返回值
-------

``String``: 计算所得哈希值.

-------
例子
-------

.. code-block:: javascript

    web3.utils.sha3('234'); // 用字符串作为参数
    > "0xc1912fee45d61c87cc5ea59dae311904cd86b84fee17cc96966216f811ce6a79"

    web3.utils.sha3(new BN('234'));
    > "0xbc36789e7a1e281436464229828f817d6612f7b477d66591ff96a9e064bcc98a"

    web3.utils.sha3(234);
    > null // 不能对数字进行哈希运算

    web3.utils.sha3(0xea); // 和上面一样，只是数字的 16 进制表示
    > null

    web3.utils.sha3('0xea'); // 先转换为一个字节数组再进行哈希运算
    > "0x2f20677459120677484f7104c76deb6846a2c071f9b3152c103bb12cd54d1a4a"


------------------------------------------------------------------------------


sha3Raw
=====================

.. code-block:: javascript

    web3.utils.sha3Raw(string)

将计算输入字符串的 sha3 哈希值，如果传入的是空字符串，将返回空字符串的哈希值而不是 ``null``

.. note::  更多详情可以查看这里 :ref:`sha3 <utils-sha3>`


------------------------------------------------------------------------------

.. _utils-soliditysha3:


soliditySha3
=====================

.. code-block:: javascript

    web3.utils.soliditySha3(param1 [, param2, ...])

使用和 solidity 同样的方式对输入参数进行 sha3 哈希运算。这意味着对这些参数在进行哈希运算之前先进行 ABI 转换和紧凑打包编码。

----------
参数
----------

1. ``paramX`` - ``Mixed``: 任意类型，或者具有 ``{type: 'uint', value: '123456'}`` 或 ``{t: 'bytes', v: '0xfff456'}`` 的对象. 基本类型将按下面的规则进行自动检测:

    - ``String`` 非数字 UTF-8 字符串会被解析为 ``string``.
    - ``String|Number|BN|HEX`` 正数会被解析为 ``uint256``.
    - ``String|Number|BN`` 负数会被解析为 ``int256``.
    - ``Boolean`` 作为 ``bool``.
    - ``String`` 以 ``0x`` 开头的 16 进制字符串会被解析为 ``bytes``.
    - ``HEX`` 16 进制表示的数字会被解析为 ``uint256``.
-------
返回值
-------

``String``: 哈希值.

-------
例子
-------

.. code-block:: javascript

    web3.utils.soliditySha3('234564535', '0xfff23243', true, -10);
    // 自动解析:        uint256,      bytes,     bool,   int256
    > "0x3e27a893dc40ef8a7f0841d96639de2f58a132be5ae466d40087a2cfa83b7179"


    web3.utils.soliditySha3('Hello!%'); // 自动解析: string
    > "0x661136a4267dba9ccdf6bfddb7c00e714de936674c4bdb065a531cf1cb15c7fc"


    web3.utils.soliditySha3('234'); // 自动解析: uint256
    > "0x61c831beab28d67d1bb40b5ae1a11e2757fa842f031a2d0bc94a7867bc5d26c2"

    web3.utils.soliditySha3(0xea); // 同上
    > "0x61c831beab28d67d1bb40b5ae1a11e2757fa842f031a2d0bc94a7867bc5d26c2"

    web3.utils.soliditySha3(new BN('234')); // 同上
    > "0x61c831beab28d67d1bb40b5ae1a11e2757fa842f031a2d0bc94a7867bc5d26c2"

    web3.utils.soliditySha3({type: 'uint256', value: '234'})); // 同上
    > "0x61c831beab28d67d1bb40b5ae1a11e2757fa842f031a2d0bc94a7867bc5d26c2"

    web3.utils.soliditySha3({t: 'uint', v: new BN('234')})); // 同上
    > "0x61c831beab28d67d1bb40b5ae1a11e2757fa842f031a2d0bc94a7867bc5d26c2"


    web3.utils.soliditySha3('0x407D73d8a49eeb85D32Cf465507dd71d507100c1');
    > "0x4e8ebbefa452077428f93c9520d3edd60594ff452a29ac7d2ccc11d47f3ab95b"

    web3.utils.soliditySha3({t: 'bytes', v: '0x407D73d8a49eeb85D32Cf465507dd71d507100c1'});
    > "0x4e8ebbefa452077428f93c9520d3edd60594ff452a29ac7d2ccc11d47f3ab95b" // 和上面一样的结果


    web3.utils.soliditySha3({t: 'address', v: '0x407D73d8a49eeb85D32Cf465507dd71d507100c1'});
    > "0x4e8ebbefa452077428f93c9520d3edd60594ff452a29ac7d2ccc11d47f3ab95b" // 同上, 如果含有大小写混合地址，还会通过校验和进行地址有效性检测，


    web3.utils.soliditySha3({t: 'bytes32', v: '0x407D73d8a49eeb85D32Cf465507dd71d507100c1'});
    > "0x3c69a194aaf415ba5d6afca734660d0a3d45acdc05d54cd1ca89a8988e7625b4" // 不同的结果


    web3.utils.soliditySha3({t: 'string', v: 'Hello!%'}, {t: 'int8', v:-23}, {t: 'address', v: '0x85F43D8a49eeB85d32Cf465507DD71d507100C1d'});
    > "0xa13b31627c1ed7aaded5aecec71baf02fe123797fffd45e662eac8e06fbe4955"



------------------------------------------------------------------------------

.. _utils-soliditysha3Raw:


soliditySha3Raw
=====================

.. code-block:: javascript

    web3.utils.soliditySha3Raw(param1 [, param2, ...])

使用和 solidity 同样的方式对输入参数进行 sha3 哈希运算。这意味着对这些参数在进行哈希运算之前先进行 ABI 转换和紧凑打包编码。
和 ``soliditySha3`` 不同的是，如果传入的是空字符串，将返回空字符串的哈希值而不是 ``null``

.. note::  更多详情可以查看这里 :ref:`soliditySha3 <utils-soliditysha3>`


------------------------------------------------------------------------------

isHex
=====================

.. code-block:: javascript

    web3.utils.isHex(hex)

判断给定的字符串是否是个 16 进制字符串。

----------
参数
----------

1. ``hex`` - ``String|HEX``: 给定的 16 进制字符串.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.utils.isHex('0xc1912');
    > true

    web3.utils.isHex(0xc1912);
    > true

    web3.utils.isHex('c1912');
    > true

    web3.utils.isHex(345);
    > true // 这里有点儿诡异，345 既可以看作一个 16 进制表示也可以当做一般数字，数字前面不带 0x 时要特别小心！

    web3.utils.isHex('0xZ1912');
    > false

    web3.utils.isHex('Hello');
    > false

------------------------------------------------------------------------------

isHexStrict
=====================

.. code-block:: javascript

    web3.utils.isHexStrict(hex)

判断给定的字符串是否是一个 16 进制字符串. 和 ``web3.utils.isHex()`` 不同的是它要求 16 进制字符串必须以 ``0x`` 开头.

----------
参数
----------

1. ``hex`` - ``String|HEX``: 给定的 16 进制字符串.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.utils.isHexStrict('0xc1912');
    > true

    web3.utils.isHexStrict(0xc1912);
    > false

    web3.utils.isHexStrict('c1912');
    > false

    web3.utils.isHexStrict(345);
    > false // 这里有点儿诡异，345 既可以看作一个 16 进制表示也可以当做一般数字，数字前面不带 0x 时要特别小心！

    web3.utils.isHexStrict('0xZ1912');
    > false

    web3.utils.isHex('Hello');
    > false

------------------------------------------------------------------------------

isAddress
=====================

.. code-block:: javascript

    web3.utils.isAddress(address)

判断给定的地址是否是一个有效的以太坊地址。大小写混合的地址还会检测校验和。

----------
Parameters
参数
----------

1. ``address`` - ``String``: 地址字符串.

-------
返回值
-------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.utils.isAddress('0xc1912fee45d61c87cc5ea59dae31190fffff232d');
    > true

    web3.utils.isAddress('c1912fee45d61c87cc5ea59dae31190fffff232d');
    > true

    web3.utils.isAddress('0XC1912FEE45D61C87CC5EA59DAE31190FFFFF232D');
    > true // 因为都是大写，不会检测校验和

    web3.utils.isAddress('0xc1912fEE45d61C87Cc5EA59DaE31190FFFFf232d');
    > true

    web3.utils.isAddress('0xC1912fEE45d61C87Cc5EA59DaE31190FFFFf232d');
    > false // 错误的校验和

------------------------------------------------------------------------------


toChecksumAddress
=====================

.. code-block:: javascript

    web3.utils.toChecksumAddress(address)

将一个只有大写或小写字符的以太坊地址转换为一个校验和地址。

----------
参数
----------

1. ``address`` - ``String``: 地址字符串.

-------
返回值
-------

``String``: 校验和地址.

-------
Example
-------

.. code-block:: javascript

    web3.utils.toChecksumAddress('0xc1912fee45d61c87cc5ea59dae31190fffff232d');
    > "0xc1912fEE45d61C87Cc5EA59DaE31190FFFFf232d"

    web3.utils.toChecksumAddress('0XC1912FEE45D61C87CC5EA59DAE31190FFFFF232D');
    > "0xc1912fEE45d61C87Cc5EA59DaE31190FFFFf232d" // 同上


------------------------------------------------------------------------------


checkAddressChecksum
=====================

.. code-block:: javascript

    web3.utils.checkAddressChecksum(address)

检测给定地址的校验和. 非校验和地址同样会返回 false.

----------
参数
----------

1. ``address`` - ``String``: 地址字符串.

-------
返回值
-------

``Boolean``: 地址的校验和有效时返回 ``true`` , 地址不是一个校验和地址或者校验和无效时返回 ``false``.

-------
例子
-------

.. code-block:: javascript

    web3.utils.checkAddressChecksum('0xc1912fEE45d61C87Cc5EA59DaE31190FFFFf232d');
    > true


------------------------------------------------------------------------------


toHex
=====================

.. code-block:: javascript

    web3.utils.toHex(mixed)

自动将给定值转换为 16 进制。Number 字符串会被解析为数字。Text 字符串会被解析为 UTF8 字符串。

----------
参数
----------

1. ``mixed`` - ``String|Number|BN|BigNumber``: 要进行 16 进制转化的输入.

-------
返回值
-------

``String``: 得到的 16 进制字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.toHex('234');
    > "0xea"

    web3.utils.toHex(234);
    > "0xea"

    web3.utils.toHex(new BN('234'));
    > "0xea"

    web3.utils.toHex(new BigNumber('234'));
    > "0xea"

    web3.utils.toHex('I have 100€');
    > "0x49206861766520313030e282ac"


------------------------------------------------------------------------------

.. _utils-tobn:

toBN
=====================

.. code-block:: javascript

    web3.utils.toBN(number)

将任何给定值安全转换为 (包括 `BigNumber.js <http://mikemcl.github.io/bignumber.js/>`_ 实例) 一个 `BN.js <https://github.com/indutny/bn.js/>`_ 实例, 以便于在 JavaScript 中处理大数.

.. note:: 如果只是 `BN.js <https://github.com/indutny/bn.js/>`_ 类，可以直接使用 :ref:`utils.BN <utils-bn>`

----------
参数
----------

1. ``number`` - ``String|Number|HEX``: 要转换为大数类型的数字.

-------
返回值
-------

``Object``: `BN.js <https://github.com/indutny/bn.js/>`_ 实例.

-------
例子
-------

.. code-block:: javascript

    web3.utils.toBN(1234).toString();
    > "1234"

    web3.utils.toBN('1234').add(web3.utils.toBN('1')).toString();
    > "1235"

    web3.utils.toBN('0xea').toString();
    > "234"


------------------------------------------------------------------------------


hexToNumberString
=====================

.. code-block:: javascript

    web3.utils.hexToNumberString(hex)

返回 16 进制字符串的数字表示

----------
参数
----------

1. ``hexString`` - ``String|HEX``: 16 进制字符串.

-------
返回值
-------

``String``: 数字字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.hexToNumberString('0xea');
    > "234"


------------------------------------------------------------------------------

hexToNumber
=====================

.. code-block:: javascript

    web3.utils.hexToNumber(hex)
    web3.utils.toDecimal(hex) // 别名, 已不推荐使用

返回 16 进制字符串的数字表示

.. note:: 这个对大数不适用, 大数应该使用 :ref:`utils.toBN <utils-tobn>`.

----------
参数
----------

1. ``hexString`` - ``String|HEX``: 16 进制字符串.

-------
返回值
-------

``Number``

-------
例子
-------

.. code-block:: javascript

    web3.utils.hexToNumber('0xea');
    > 234


------------------------------------------------------------------------------

numberToHex
=====================

.. code-block:: javascript

    web3.utils.numberToHex(number)
    web3.utils.fromDecimal(number) // 别名, 不推荐使用

返回给定数字的 16 进制表示

----------
参数
----------

1. ``number`` - ``String|Number|BN|BigNumber``: 数字或数字字符串.

-------
返回值
-------

``String``: 给定数字的 16 进制表示.

-------
例子
-------

.. code-block:: javascript

    web3.utils.numberToHex('234');
    > '0xea'


------------------------------------------------------------------------------


hexToUtf8
=====================

.. code-block:: javascript

    web3.utils.hexToUtf8(hex)
    web3.utils.hexToString(hex) // 别名
    web3.utils.toUtf8(hex) // 别名, 不推荐使用

返回给定 16 进制数字的 UTF-8 字符串表示。

----------
参数
----------

1. ``hex`` - ``String``: 要转换为 UTF-8 字符串的 16 进制字符串.

-------
返回值
-------

``String``: UTF-8 字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.hexToUtf8('0x49206861766520313030e282ac');
    > "I have 100€"


------------------------------------------------------------------------------

hexToAscii
=====================

.. code-block:: javascript

    web3.utils.hexToAscii(hex)
    web3.utils.toAscii(hex) // 别名, 已不推荐使用

返回给定 16 进制数字的 ASCII 码字符串表示

----------
参数
----------

1. ``hex`` - ``String``: 要转换为 ASCII 码字符串的 16 进制字符串.

-------
返回值
-------

``String``: ASCII 码字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.hexToAscii('0x4920686176652031303021');
    > "I have 100!"


------------------------------------------------------------------------------

.. _utils-utf8tohex:

utf8ToHex
=====================

.. code-block:: javascript

    web3.utils.utf8ToHex(string)
    web3.utils.stringToHex(string) // 别名
    web3.utils.fromUtf8(string) // 别名, 不推荐使用

返回给定 UTF-8 字符串的 16 进制表示。

----------
参数
----------

1. ``string`` - ``String``: 要转换为 16 进制字符串的 UTF-8 字符串.

-------
返回值
-------

``String``: 16 进制字符串.


-------
例子
-------

.. code-block:: javascript

    web3.utils.utf8ToHex('I have 100€');
    > "0x49206861766520313030e282ac"


------------------------------------------------------------------------------

asciiToHex
=====================

.. code-block:: javascript

    web3.utils.asciiToHex(string)
    web3.utils.fromAscii(string) // 别名, 不推荐使用


返回给定 ASCII 码字符串的 16 进制表示

----------
参数
----------

1. ``string`` - ``String``: 要转换为 16 进制字符串的 ASCII 码字符串.

-------
返回值
-------

``String``: 16 进制字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.asciiToHex('I have 100!');
    > "0x4920686176652031303021"


------------------------------------------------------------------------------

hexToBytes
=====================

.. code-block:: javascript

    web3.utils.hexToBytes(hex)

返回给定 16 进制字符串的字节数组表示。

----------
参数
----------

1. ``hex`` - ``String|HEX``: 要进行转换的 16 进制串.

-------
返回值
-------

``Array``: 字节数组.

-------
例子
-------

.. code-block:: javascript

    web3.utils.hexToBytes('0x000000ea');
    > [ 0, 0, 0, 234 ]

    web3.utils.hexToBytes(0x000000ea);
    > [ 234 ]


------------------------------------------------------------------------------


bytesToHex
=====================

.. code-block:: javascript

    web3.utils.bytesToHex(byteArray)

返回一个字节数组的 16 进制字符串表示

----------
参数
----------

1. ``byteArray`` - ``Array``: 要进行转换的字节数组.

-------
返回值
-------

``String``: 16 进制字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.bytesToHex([ 72, 101, 108, 108, 111, 33, 36 ]);
    > "0x48656c6c6f2125"



------------------------------------------------------------------------------

toWei
=====================

.. code-block:: javascript

    web3.utils.toWei(number [, unit])


将任意 `ether <http://ethdocs.org/en/latest/ether.html>`_ 值转换为 `wei <http://ethereum.stackexchange.com/questions/253/the-ether-denominations-are-called-finney-szabo-and-wei-what-who-are-these-na>`_.

.. note:: "wei" 是最小的以太币单位, 你应该总是用 wei 进行数值计算，仅由于显示原因进行换算.

----------
参数
----------

1. ``number`` - ``String|BN``: 要转换的数字.
2. ``unit`` - ``String`` (optional, defaults to ``"ether"``): 要转换的以太币单位. 支持的单位包括:
    - ``noether``: '0'
    - ``wei``: '1'
    - ``kwei``: '1000'
    - ``Kwei``: '1000'
    - ``babbage``: '1000'
    - ``femtoether``: '1000'
    - ``mwei``: '1000000'
    - ``Mwei``: '1000000'
    - ``lovelace``: '1000000'
    - ``picoether``: '1000000'
    - ``gwei``: '1000000000'
    - ``Gwei``: '1000000000'
    - ``shannon``: '1000000000'
    - ``nanoether``: '1000000000'
    - ``nano``: '1000000000'
    - ``szabo``: '1000000000000'
    - ``microether``: '1000000000000'
    - ``micro``: '1000000000000'
    - ``finney``: '1000000000000000'
    - ``milliether``: '1000000000000000'
    - ``milli``: '1000000000000000'
    - ``ether``: '1000000000000000000'
    - ``kether``: '1000000000000000000000'
    - ``grand``: '1000000000000000000000'
    - ``mether``: '1000000000000000000000000'
    - ``gether``: '1000000000000000000000000000'
    - ``tether``: '1000000000000000000000000000000'

-------
返回值
-------

``String|BN``: 给一个数字字符串就返回一个字符串, 否则返回一个 `BN.js <https://github.com/indutny/bn.js/>`_ 实例.

-------
例子
-------

.. code-block:: javascript

    web3.utils.toWei('1', 'ether');
    > "1000000000000000000"

    web3.utils.toWei('1', 'finney');
    > "1000000000000000"

    web3.utils.toWei('1', 'szabo');
    > "1000000000000"

    web3.utils.toWei('1', 'shannon');
    > "1000000000"



------------------------------------------------------------------------------

fromWei
=====================

.. code-block:: javascript

    web3.utils.fromWei(number [, unit])


将任意数量的 `wei <http://ethereum.stackexchange.com/questions/253/the-ether-denominations-are-called-finney-szabo-and-wei-what-who-are-these-na>`_ 转换为 `ether <http://ethdocs.org/en/latest/ether.html>`_.

.. note:: "wei" 是最小的以太币单位, 你应该总是用 wei 进行数值计算，仅由于显示原因进行换算.

----------
参数
----------

1. ``number`` - ``String|BN``: 以 wei 来表示的数字.

2. ``unit`` - ``String`` (可选参数, 默认为 ``"ether"``): 要转换到的以太币单位. 可能支持的单位包括:
    - ``noether``: '0'
    - ``wei``: '1'
    - ``kwei``: '1000'
    - ``Kwei``: '1000'
    - ``babbage``: '1000'
    - ``femtoether``: '1000'
    - ``mwei``: '1000000'
    - ``Mwei``: '1000000'
    - ``lovelace``: '1000000'
    - ``picoether``: '1000000'
    - ``gwei``: '1000000000'
    - ``Gwei``: '1000000000'
    - ``shannon``: '1000000000'
    - ``nanoether``: '1000000000'
    - ``nano``: '1000000000'
    - ``szabo``: '1000000000000'
    - ``microether``: '1000000000000'
    - ``micro``: '1000000000000'
    - ``finney``: '1000000000000000'
    - ``milliether``: '1000000000000000'
    - ``milli``: '1000000000000000'
    - ``ether``: '1000000000000000000'
    - ``kether``: '1000000000000000000000'
    - ``grand``: '1000000000000000000000'
    - ``mether``: '1000000000000000000000000'
    - ``gether``: '1000000000000000000000000000'
    - ``tether``: '1000000000000000000000000000000'

-------
返回值
-------

``String``: 数字字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.fromWei('1', 'ether');
    > "0.000000000000000001"

    web3.utils.fromWei('1', 'finney');
    > "0.000000000000001"

    web3.utils.fromWei('1', 'szabo');
    > "0.000000000001"

    web3.utils.fromWei('1', 'shannon');
    > "0.000000001"

------------------------------------------------------------------------------

unitMap
=====================

.. code-block:: javascript

    web3.utils.unitMap


显示所有的 `ether 单位 <http://ethdocs.org/en/latest/ether.html>`_ 和它们对应的 `wei <http://ethereum.stackexchange.com/questions/253/the-ether-denominations-are-called-finney-szabo-and-wei-what-who-are-these-na>`_ 数量.

----------
返回值
----------

- 带有下面属性的 ``Object``:
    - ``noether``: '0'
    - ``wei``: '1'
    - ``kwei``: '1000'
    - ``Kwei``: '1000'
    - ``babbage``: '1000'
    - ``femtoether``: '1000'
    - ``mwei``: '1000000'
    - ``Mwei``: '1000000'
    - ``lovelace``: '1000000'
    - ``picoether``: '1000000'
    - ``gwei``: '1000000000'
    - ``Gwei``: '1000000000'
    - ``shannon``: '1000000000'
    - ``nanoether``: '1000000000'
    - ``nano``: '1000000000'
    - ``szabo``: '1000000000000'
    - ``microether``: '1000000000000'
    - ``micro``: '1000000000000'
    - ``finney``: '1000000000000000'
    - ``milliether``: '1000000000000000'
    - ``milli``: '1000000000000000'
    - ``ether``: '1000000000000000000'
    - ``kether``: '1000000000000000000000'
    - ``grand``: '1000000000000000000000'
    - ``mether``: '1000000000000000000000000'
    - ``gether``: '1000000000000000000000000000'
    - ``tether``: '1000000000000000000000000000000'


-------
例子
-------

.. code-block:: javascript

    web3.utils.unitMap
    > {
        noether: '0',
        wei:        '1',
        kwei:       '1000',
        Kwei:       '1000',
        babbage:    '1000',
        femtoether: '1000',
        mwei:       '1000000',
        Mwei:       '1000000',
        lovelace:   '1000000',
        picoether:  '1000000',
        gwei:       '1000000000',
        Gwei:       '1000000000',
        shannon:    '1000000000',
        nanoether:  '1000000000',
        nano:       '1000000000',
        szabo:      '1000000000000',
        microether: '1000000000000',
        micro:      '1000000000000',
        finney:     '1000000000000000',
        milliether: '1000000000000000',
        milli:      '1000000000000000',
        ether:      '1000000000000000000',
        kether:     '1000000000000000000000',
        grand:      '1000000000000000000000',
        mether:     '1000000000000000000000000',
        gether:     '1000000000000000000000000000',
        tether:     '1000000000000000000000000000000'
    }

------------------------------------------------------------------------------

padLeft
=====================

.. code-block:: javascript

    web3.utils.padLeft(string, characterAmount [, sign])
    web3.utils.leftPad(string, characterAmount [, sign]) // 别名


对一个字符串进行左补齐, 在需要对 16 进制字符串进行填充是很有用.

----------
参数
----------

1. ``string`` - ``String``: 需要左补齐的字符串.
2. ``characterAmount`` - ``Number``: 整个字符串所含有的字符数量.
3. ``sign`` - ``String`` (可选): 要使用的字符符号, 默认为 ``"0"``.

-------
返回值
-------

``String``: 补齐后的字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.padLeft('0x3456ff', 20);
    > "0x000000000000003456ff"

    web3.utils.padLeft(0x3456ff, 20);
    > "0x000000000000003456ff"

    web3.utils.padLeft('Hello', 20, 'x');
    > "xxxxxxxxxxxxxxxHello"

------------------------------------------------------------------------------

padRight
=====================

.. code-block:: javascript

    web3.utils.padRight(string, characterAmount [, sign])
    web3.utils.rightPad(string, characterAmount [, sign]) // 别名


对一个字符串进行右补齐，在需要对 16 进制字符串进行补齐的适合很有用。

----------
参数
----------

1. ``string`` - ``String``: 需要进行右补齐的字符串.
2. ``characterAmount`` - ``Number``: 整个字符串应该包含的字符数量.
3. ``sign`` - ``String`` (可选): 要使用的字符符号, 默认为 ``"0"``.

-------
返回值
-------

``String``: 补齐后的字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.padRight('0x3456ff', 20);
    > "0x3456ff00000000000000"

    web3.utils.padRight(0x3456ff, 20);
    > "0x3456ff00000000000000"

    web3.utils.padRight('Hello', 20, 'x');
    > "Helloxxxxxxxxxxxxxxx"

------------------------------------------------------------------------------

toTwosComplement
=====================

.. code-block:: javascript

    web3.utils.toTwosComplement(number)


将一个负数转换为补码表示。

----------
参数
----------

1. ``number`` - ``Number|String|BigNumber``: 需要进行转换的数字.

-------
返回值
-------

``String``: 转换后的 16 进制字符串.

-------
例子
-------

.. code-block:: javascript

    web3.utils.toTwosComplement('-1');
    > "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"

    web3.utils.toTwosComplement(-1);
    > "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"

    web3.utils.toTwosComplement('0x1');
    > "0x0000000000000000000000000000000000000000000000000000000000000001"

    web3.utils.toTwosComplement(-15);
    > "0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff1"

    web3.utils.toTwosComplement('-0x1');
    > "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"

