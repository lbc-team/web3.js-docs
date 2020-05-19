.. _eth-abi:

============
web3.eth.abi
============


``web3.eth.abi`` 函数用来解码及编码为 ABI (Application Binary Interface应用程序二进制接口) 以用于 EVM（以太坊虚拟机）进行函数调用。


------------------------------------------------------------------------------


encodeFunctionSignature
========================

.. code-block:: javascript

    web3.eth.abi.encodeFunctionSignature(functionName);

将函数名称编码为其ABI签名，即函数名称（包括参数类型）的sha3哈希的前4个字节。


----------
参数
----------

1. ``functionName`` - ``String|Object``: 需要编码的函数名字符串或者函数 :ref:`JSON 接口 <glossary-json-interface>` 对象。
 如果是字符串，这形式需要是 ``function(type,type,...)``,例如： ``myFunction(uint256,uint32[],bytes10,bytes)``

-------
返回值
-------

``String`` - 函数的ABI签名。

-------
示例
-------

.. code-block:: javascript

    // 参数是 JSON 接口对象时
    web3.eth.abi.encodeFunctionSignature({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    })
    > 0x24ee0097

    // 参数是字符串
    web3.eth.abi.encodeFunctionSignature('myMethod(uint256,string)')
    > '0x24ee0097'


------------------------------------------------------------------------------

encodeEventSignature
=====================

.. code-block:: javascript

    web3.eth.abi.encodeEventSignature(eventName);

将事件名称编码为其ABI签名，该签名是事件名称包括输入参数类型的sha3哈希。


----------
参数
----------

1. ``eventName`` - ``String|Object``: 需要编码的事件名字符串或者事件 :ref:`JSON 接口 <glossary-json-interface>` 对象。
 如果是字符串，这形式需要是 ``event(type,type,...)``，如： ``myEvent(uint256,uint32[],bytes10,bytes)``

-------
返回值
-------

``String`` - 事件的ABI签名。

-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.encodeEventSignature('myEvent(uint256,bytes32)')
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97

    // 或者 ABI 接口对象形式
    web3.eth.abi.encodeEventSignature({
        name: 'myEvent',
        type: 'event',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'bytes32',
            name: 'myBytes'
        }]
    })
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97


------------------------------------------------------------------------------

encodeParameter
=====================

.. code-block:: javascript

    web3.eth.abi.encodeParameter(type, parameter);


根据参数的类型将参数编码为其ABI表示形式。


----------
参数
----------

1. ``type`` - ``String|Object``: 参数的类型， 参考 `solidity 文档 <http://solidity.readthedocs.io/en/develop/types.html>`_  查看类型列表。（译者注，Solidity 中文文档 可以在`这里查看 <https://learnblockchain.cn/docs/solidity/types.html>`_ 。
2. ``parameter`` - ``Mixed``: 要编码的实际参数。

-------
返回值
-------

``String`` - ABI编码后的参数。


-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    web3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    web3.eth.abi.encodeParameter('bytes32', '0xdf3234');
    > "0xdf32340000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter('bytes', '0xdf3234');
    > "0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000003df32340000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter('bytes32[]', ['0xdf3234', '0xfdfd']);
    > "00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000002df32340000000000000000000000000000000000000000000000000000000000fdfd000000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter(
        {
            "ParentStruct": {
                "propertyOne": 'uint256',
                "propertyTwo": 'uint256',
                "childStruct": {
                    "propertyOne": 'uint256',
                    "propertyTwo": 'uint256'
                }
            }
        },
        {
            "propertyOne": 42,
            "propertyTwo": 56,
            "childStruct": {
                "propertyOne": 45,
                "propertyTwo": 78
            }
        }
    );
    > "0x000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e"
------------------------------------------------------------------------------

encodeParameters
=====================

.. code-block:: javascript

    web3.eth.abi.encodeParameters(typesArray, parameters);

基于 :ref:`JSON 接口 <glossary-json-interface>` 对象编码函数参数。

----------
参数
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: 一个函数的类型的数组或者 :ref:`JSON 接口 <glossary-json-interface>` 。参考 `solidity 文档 <http://solidity.readthedocs.io/en/develop/types.html>`_  查看类型列表。（译者注，Solidity 中文文档 可以在`这里查看 <https://learnblockchain.cn/docs/solidity/types.html>`_ 。
2. ``parameters`` - ``Array``: 需要编码的参数

-------
返回值
-------

``String`` - ABI编码后的参数。

-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.encodeParameters(['uint256','string'], ['2345675643', 'Hello!%']);
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameters(['uint8[]','bytes32'], [['34','434'], '0x324567fff']);
    > "0x0

    web3.eth.abi.encodeParameters(
        [
            'uint8[]',
            {
                "ParentStruct": {
                    "propertyOne": 'uint256',
                    "propertyTwo": 'uint256',
                    "ChildStruct": {
                        "propertyOne": 'uint256',
                        "propertyTwo": 'uint256'
                    }
                }
            }
        ],
        [
            ['34','434'],
            {
                "propertyOne": '42',
                "propertyTwo": '56',
                "ChildStruct": {
                    "propertyOne": '45',
                    "propertyTwo": '78'
                }
            }
        ]
    );
    > "0x00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000018"

------------------------------------------------------------------------------

encodeFunctionCall
=====================

.. code-block:: javascript

    web3.eth.abi.encodeFunctionCall(jsonInterface, parameters);

用 :ref:`JSON 接口 <glossary-json-interface>` 及给定的参数编码函数调用

----------
参数
----------

1. ``jsonInterface`` - ``Object``: 函数的 :ref:`JSON 接口 <glossary-json-interface>` 对象
2. ``parameters`` - ``Array``: 需要编码的参数

-------
返回值
-------

``String`` - ABI编码的函数调用。 表示为函数签名+参数。 

-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.encodeFunctionCall({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    }, ['2345675643', 'Hello!%']);
    > "0x24ee0097000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

------------------------------------------------------------------------------

decodeParameter
=====================

.. code-block:: javascript

    web3.eth.abi.decodeParameter(type, hexString);

将ABI编码参数解码为其对应的JavaScript类型。


----------
参数
----------

1. ``type`` - ``String|Object``: 参数的类型, 参考 `solidity 文档 <http://solidity.readthedocs.io/en/develop/types.html>`_  查看类型列表。（译者注，Solidity 中文文档可以在`这里查看 <https://learnblockchain.cn/docs/solidity/types.html>`_ 。 
2. ``hexString`` - ``String``: 要解码的ABI字节码。

-------
返回值
-------

``Mixed`` - 解码后的参数。

-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.decodeParameter('uint256', '0x0000000000000000000000000000000000000000000000000000000000000010');
    > "16"

    web3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"

    web3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"

    web3.eth.abi.decodeParameter(
        {
            "ParentStruct": {
              "propertyOne": 'uint256',
              "propertyTwo": 'uint256',
              "childStruct": {
                "propertyOne": 'uint256',
                "propertyTwo": 'uint256'
              }
            }
        },

    , '0x000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e');
    > {
        '0': {
            '0': '42',
            '1': '56',
            '2': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'childStruct': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'propertyOne': '42',
            'propertyTwo': '56'
        },
        'ParentStruct': {
            '0': '42',
            '1': '56',
            '2': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'childStruct': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'propertyOne': '42',
            'propertyTwo': '56'
        }
    }

------------------------------------------------------------------------------

decodeParameters
=====================

.. code-block:: javascript

    web3.eth.abi.decodeParameters(typesArray, hexString);

将ABI编码参数解码为其对应JavaScript类型。


----------
参数
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: 类型数组或者:ref:`JSON 接口 <glossary-json-interface>` 输出数组。 参考 `solidity 文档 <http://solidity.readthedocs.io/en/develop/types.html>`_  查看类型列表。（译者注，Solidity 中文文档可以在`这里查看 <https://learnblockchain.cn/docs/solidity/types.html>`_ 。 
2. ``hexString`` - ``String``: 要解码的ABI字节码。

-------
返回值
-------

``Object`` - 已解码参数的结果对象。

-------
示例
-------

.. code-block:: javascript

    web3.eth.abi.decodeParameters(['string', 'uint256'], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result { '0': 'Hello!%!', '1': '234' }

    web3.eth.abi.decodeParameters([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber'
    }], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result {
        '0': 'Hello!%!',
        '1': '234',
        myString: 'Hello!%!',
        myNumber: '234'
    }

    web3.eth.abi.decodeParameters([
      'uint8[]',
      {
        "ParentStruct": {
          "propertyOne": 'uint256',
          "propertyTwo": 'uint256',
          "childStruct": {
            "propertyOne": 'uint256',
            "propertyTwo": 'uint256'
          }
        }
      }
    ], '0x00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000018');
    > Result {
        '0': ['42', '24'],
        '1': {
            '0': '42',
            '1': '56',
            '2':
                {
                    '0': '45',
                    '1': '78',
                    'propertyOne': '45',
                    'propertyTwo': '78'
                },
            'childStruct':
                {
                    '0': '45',
                    '1': '78',
                    'propertyOne': '45',
                    'propertyTwo': '78'
                },
            'propertyOne': '42',
            'propertyTwo': '56'
        }
    }

------------------------------------------------------------------------------


decodeLog
=====================

.. code-block:: javascript

    web3.eth.abi.decodeLog(inputs, hexString, topics);

解码ABI编码的日志数据和索引主题数据。


----------
参数
----------

1. ``inputs`` - ``Object``:  :ref:`JSON 接口 <glossary-json-interface>` 输入数组. 参考 `solidity 文档 <http://solidity.readthedocs.io/en/develop/types.html>`_  查看类型列表。（译者注，Solidity 中文文档可以在`这里查看 <https://learnblockchain.cn/docs/solidity/types.html>`_ 。 
2. ``hexString`` - ``String``: 日志的 ``data`` 字段中的 ABI 字节码 。
3. ``topics`` - ``Array``: 日志中带有索引主题参数的数组，如果为非匿名事件，则不包含topic[0]，否则包含topic[0]。


-------
返回值
-------

``Object`` - 包含已解码参数的结果对象。

-------
示例
-------

.. code-block:: javascript


    web3.eth.abi.decodeLog([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber',
        indexed: true
    },{
        type: 'uint8',
        name: 'mySmallNumber',
        indexed: true
    }],
    '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000748656c6c6f252100000000000000000000000000000000000000000000000000',
    ['0x000000000000000000000000000000000000000000000000000000000000f310', '0x0000000000000000000000000000000000000000000000000000000000000010']);
    > Result {
        '0': 'Hello%!',
        '1': '62224',
        '2': '16',
        myString: 'Hello%!',
        myNumber: '62224',
        mySmallNumber: '16'
    }
