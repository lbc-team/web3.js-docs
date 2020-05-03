.. _eth-iban:

=========
web3.eth.Iban
=========

``web3.eth.Iban`` 相关函数让我们可以将以太坊地址和 IBAN/BBAN 地址之间相互转换。

------------------------------------------------------------------------------

Iban 实例
=====================

这个就是 Iban 实例

.. code-block:: javascript

    > Iban { _iban: 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS' }

------------------------------------------------------------------------------

Iban 构造器
=====================

.. code-block:: javascript

    new web3.eth.Iban(ibanAddress)

生成带有转换方法和有效性检查的 iban 对象。其它带有转换功能的单例函数如下：
:ref:`Iban.toAddress() <_eth-iban-toaddress>`,
:ref:`Iban.toIban() <_eth-iban-toiban>`,
:ref:`Iban.fromAddress() <_eth-iban-fromaddress>`,
:ref:`Iban.fromBban() <_eth-iban-frombban>`,
:ref:`Iban.createIndirect() <_eth-iban-createindirect>`,
:ref:`Iban.isValid() <_eth-iban-isvalid>`.

----------
参数
----------

1.``String``：用来创建 Iban 实例的 IBAN 地址字符串。

-------
返回值
-------

``Object`` - Iban 实例。

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS");
    > Iban { _iban: 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS' }


------------------------------------------------------------------------------

.. _eth-iban-toaddress:

toAddress
=====================

    静态函数

.. code-block:: javascript

    web3.eth.Iban.toAddress(ibanAddress)

单例: 将 direct IBAN 地址转换为以太坊地址。

.. note:: IBAN 实例对象也有此方法。

----------
参数
----------

1.``String``：需要转换的 IBAN 地址。

-------
返回值
-------

``String`` - 以太坊地址。

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.toAddress("XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS");
    > "0x00c5496aEe77C1bA1f0854206A26DdA82a81D6D8"


------------------------------------------------------------------------------

.. _eth-iban-toiban:

toIban
=====================

    静态函数

.. code-block:: javascript

    web3.eth.Iban.toIban(address)

单例: 将以太坊地址转换为 direct IBAN 地址。

----------
参数
----------

1. ``String``: 需要进行转换的以太地址。

----------
返回值
----------

``String`` - IBAN 地址。

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.toIban("0x00c5496aEe77C1bA1f0854206A26DdA82a81D6D8");
    > "XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS"


------------------------------------------------------------------------------

.. _eth-iban-fromaddress:

    静态函数, 返回 IBAN 实例

fromAddress
=====================

.. code-block:: javascript

    web3.eth.Iban.fromAddress(address)

单例：将以太地址转换为 direct IBAN 实例。

----------
参数
----------

1. ``String``: 需要进行转换的以太地址。

----------
返回值
----------

``Object`` - IBAN 实例。

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.fromAddress("0x00c5496aEe77C1bA1f0854206A26DdA82a81D6D8");
    > Iban {_iban: "XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS"}


------------------------------------------------------------------------------

.. _eth-iban-frombban:

    静态函数, 返回 IBAN 实例。

fromBban
=====================

.. code-block:: javascript

    web3.eth.Iban.fromBban(bbanAddress)

单例: 将 BBAN 地址转换为 direct IBAN 实例。

----------
参数
----------

1. ``String``: 需要进行转换的 BBAN 地址。

----------
返回值
----------

``Object`` - IBAN 实例。

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.fromBban('ETHXREGGAVOFYORK');
    > Iban {_iban: "XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS"}


------------------------------------------------------------------------------

.. _eth-iban-createindirect:

    静态函数, 返回 IBAN 实例。

createIndirect
=====================

.. code-block:: javascript

    web3.eth.Iban.createIndirect(options)

单例: 根据机构和标识符创建 indirect IBAN 地址。

----------
参数
----------

1. ``Object``: 包含下面属性的 options 对象:
    - ``institution`` - ``String``: 分配的机构名称
    - ``identifier`` - ``String``: 分配的标识符

----------
返回值
----------

``Object`` - IBAN 实例。

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.createIndirect({
        institution: "XREG",
        identifier: "GAVOFYORK"
    });
    > Iban {_iban: "XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS"}


------------------------------------------------------------------------------

.. _eth-iban-isvalid:

    静态函数, 返回布尔值

isValid
=====================

.. code-block:: javascript

    web3.eth.Iban.isValid(ibanAddress)

单例: 检查 IBAN 地址是否有效。

.. note:: IBAN 实例也包含该方法。

----------
参数
----------

1. ``String``: 需要进行检查的 IBAN 地址。

----------
返回值
----------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    web3.eth.Iban.isValid("XE81ETHXREGGAVOFYORK");
    > true

    web3.eth.Iban.isValid("XE82ETHXREGGAVOFYORK");
    > false // 因为校验值不对


------------------------------------------------------------------------------

prototype.isValid
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.isValid()

单例: 检查 IBAN 地址是否有效

.. note:: 该方法也存在于 IBAN 实例中。

----------
参数
----------

1. ``String``: 需要检查的 IBAN 地址。

----------
返回值
----------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.isValid();
    > true


------------------------------------------------------------------------------

prototype.isDirect
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.isDirect()

检查当前 IBAN 实例是否使用了 direct 编码方式。
----------
参数
----------

无

----------
返回值
----------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.isDirect();
    > false

------------------------------------------------------------------------------

prototype.isIndirect
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.isIndirect()

检查当前 IBAN 实例是否使用了 indirect 编码方式。

----------
参数
----------

无

----------
返回值
----------

``Boolean``

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.isIndirect();
    > true

------------------------------------------------------------------------------

prototype.checksum
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.checksum()

返回 IBAN 实例校验和。

----------
参数
----------

无

----------
返回值
----------

``String``: IBAN 校验和。

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.checksum();
    > "81"


------------------------------------------------------------------------------

prototype.institution
=====================

    Iban 实例方法


.. code-block:: javascript

    web3.eth.Iban.prototype.institution()

返回 IBAN 实例对应的机构名称。

----------
参数
----------

无

----------
返回值
----------

``String``: IBAN 的机构名称。

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.institution();
    > 'XREG'


------------------------------------------------------------------------------

prototype.client
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.client()

返回 IBAN 实例的客户标识。

----------
参数
----------

无

----------
返回值
----------

``String``: IBAN 客户标识

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban("XE81ETHXREGGAVOFYORK");
    iban.client();
    > 'GAVOFYORK'

------------------------------------------------------------------------------

prototype.toAddress
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.toString()

返回 IBAN 实例的以太坊地址。

----------
参数
----------

无

----------
返回值
----------

``String``: 以太坊地址

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
    iban.toAddress();
    > '0x00c5496aEe77C1bA1f0854206A26DdA82a81D6D8'


------------------------------------------------------------------------------

prototype.toString
=====================

    Iban 实例方法

.. code-block:: javascript

    web3.eth.Iban.prototype.toString()

返回 IBAN 实例的 IBAN 地址。

----------
参数
----------

无

----------
返回值
----------

``String``: IBAN 地址。

-------
例子
-------

.. code-block:: javascript

    var iban = new web3.eth.Iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
    iban.toString();
    > 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS'

