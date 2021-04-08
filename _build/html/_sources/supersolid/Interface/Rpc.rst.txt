联盟链RPC介绍
-----------------------------

RPC是指联盟链对外的rpc接口，开发人员可利用这些接口获取联盟链的数据。当前 :doc:`SSN <../Definitions>` 和SSB都有RPC接口。 开发人员可直接利用这些接口做二次开发。

另外两种 :doc:`Console <./Console>` 和 :doc:`SDK <./Sdk>` 的开发模式，将在本章其他节介绍。

启动RPC的方式
>>>>>>>>>>>>>>>>>>>>>>>>>>

.. |br| raw:: html

    <br/>

|br|

**启动SSB RPC的方式**

在 :doc:`联盟链部署 <../Supersolid/Deploy>` 中，SSB的启动方式如下
::
    super-solid-base --verbosity=4, --rpc, --networkid=1510, --datadir=./path/to/chaindata, 
    --rpcaddr=0.0.0.0, --rpcport=8545, --rpcapi=chain3,mc,net,db

其中，--rpc、--rpcaddr和--rpcport就是指明RPC启动的三个参数，开发人员可选择为需要RPC服务的SSB加上这三个启动参数。

**启动SSN RPC的方式**

同样，在 :doc:`联盟链部署 <../Supersolid/Deploy>` 中，SSN的启动方式如下
::
    super-solid-node --rpc --rpcaddr 0.0.0.0 --rpcport 8546 --p2pport 30383

同样，可选择以上三个参数来为SSN开启RPC服务。


SSN RPC命令汇总
>>>>>>>>>>>>>>>>>>>>>>>>>>
联盟链主要是在SSN上进行二次开发，在开启SSN RPC服务后，即可用以下常用的RPC服务。

假设当前rpcaddr=127.0.0.1，rpcport=8546。

|br|

mh
:::::::::::::::::::::::

getSSNId_： 获取SSN节点id

sendRawTransaction_: 发送已加签好的交易

accounts_：获取SSN节点已创建地址列表 

sign_：以特殊前缀加签 

signTransaction_：交易加签

sendTransaction_：以未加签方式发送交易

anyCall_：调用联盟链已部署合约方法，如何部署联盟链合约，请参见

getBalance_：获取联盟链账号余额

getBlock_：获取联盟链某一区块信息

getBlockList_：获取联盟链某段区块信息

getBlockNumber_：获取联盟链区块高度

getContractAddrList_：获取联盟链已注册合约列表

getNonce_：获得对应账号在联盟链中的Nonce

getReceiptByHash_：通过交易hash获取联盟链的tx执行结果

getReceiptByNonce_：通过账号和Nonce获取联盟链的tx执行结果

getTransactionByHash_：通过交易HASH获取联盟链的交易信息

getTransactionByNonce_：通过账号和Nonce获取联盟链的交易信息

syncing_：获取联盟链节点同步信息

getBlockThreshold_：获取联盟链下次续费块高度

|br|
|br|

.. _getSSNId:

:接口: `mh`_.getSSNId

:描述: 获取SSN节点id 

:输入参数: 无

:返回:

- SSN 地址
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getSSNId","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0xe569b8d860ae0b9......fd66eb24a78de3fed"}

-------------------------------------------------------------------------------------

.. _sendRawTransaction:

:接口: `mh`_.sendRawTransaction

:描述: 发送已加签好的交易

:输入参数: 

- 加签交易数据

:返回:

- 交易hash
 
示例: 
::
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_sendRawTransaction","params":["0xf8698080808094......ded65da7ff5426d4de44fd634cff969169617c4dca01"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0x039ace1656a1e2300aadcc......014ae9b605bcf0dc4c991db2faefeb201e"}

-------------------------------------------------------------------------------------

.. _accounts:

:接口: `mh`_.accounts

:描述: 获取SSN节点已创建地址列表 

:输入参数: 无

:返回:

- SSN 地址列表集
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_accounts","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":["0xe569b8d8......eb24a78de3fed","0x7f5440ce55db40......0b06d79defc87c7c","0xc4c17b......038af9a2d98309cd81f42b67"]}

-------------------------------------------------------------------------------------

.. _sign:

:接口: `mh`_.sign

:描述: 

以特定前缀签名,通过向消息添加前缀，可以将计算得到的签名识别为moheng特定的签名。这可以防止恶意的DApp对任意数据(如事务)进行签名，并使用签名冒充受害者。注意要签名的地址必须解锁。

:输入参数: 

- 加签账号地址
- 加签明文数据

:返回:

- 签名结果
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_sign","params":["0x7f5440ce55db4.......c0b06d79defc87c7c", "0xf869808080809......04d754ab038af9a2d98309cd8f"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0x065529a4b5f86ceacff00e10fe18279ba0bd870d2fbab84a0731285deeb4......105ead228cad3991f0ec020b82071b"}

-------------------------------------------------------------------------------------

.. _signTransaction:

:接口: `mh`_.signTransaction

:描述: 对交易进行加签，注意要签名的地址必须解锁。

:输入参数: 

- from：交易发送账号地址，也是交易加签地址
- to：交易指向的账号地址，部署合约交易不填写
- value：交易转出的货币数量，
- data：加签数据
- nonce ：交易nonce ，可不填

:返回:

- raw: 交易加签结果
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_signTransaction","params":[{"from":"0x7f5440ce55......c3ec0b06d79defc87c7c", "to":"0xc4c17b9e04......2d98309cd81f42b67", "value":"0xDE0B6B3A7640000"}]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"raw":"0xf8690180808094c4c17b9e04d......cd4edc2f1b","tx":{"TxData":{"nonce":1,"syscnt":0,"gasPrice":0,"gas":0,"to":"0xc4c17b9e04......af9a2d98309cd81f42b67","value":1000000000000000000,"input":null,"shardingFlag":0,"via":null,"v":240,"r":898862982657692024547......9182641849803035800591990157411498310,"s":507781054392481032685727479435646309......2747609317939843294788595483,"hash":null},"DirCallSent":false}}}

-------------------------------------------------------------------------------------

.. _sendTransaction:

:接口: `mh`_.sendTransaction

:描述: 发送未加签的交易，注意要签名的地址必须解锁。

:输入参数: 

- from：交易发送账号地址，也是交易加签地址
- to：交易指向的账号地址，部署合约交易不填写
- value：交易转出的货币数量，
- data：交易数据
- nonce ：交易nonce ，可不填

:返回:

- 交易hash
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_sendTransaction","params":[{"from":"0x7f5440ce55db403e......b06d79defc87c7c", "to":"0xc4c17b9e04d......d98309cd81f42b67", "value":"0xDE0B6B3A7640000"}]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0x8c6b2a2c1392839d04223bc3......c7c70e4ebf62abee024a04f47f"}

-------------------------------------------------------------------------------------

.. _anyCall:

:接口: `mh`_.anyCall

:描述: 获取SSN合约函数的返回值，调用此接口前必须将合约注册入dappbase

:输入参数: 

-  Sender：查询账号
-  DappAddr:联盟链业务逻辑地址
-  Params： 第一个参数是调用的方法，之后是方法传入参数

:返回:

- 函数返回值
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_anyCall","params":[{"DappAddr":"0x974c37d2b3a7......b94285cf5126512a", "Params":["coinName"]}]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"\"test coin\""}

-------------------------------------------------------------------------------------

.. _getBalance:

:接口: `mh`_.getBalance

:描述: 获得对应账号在联盟链中的货币余额。

:输入参数: 

-  账号地址

:返回:

- 账号余额，精度18，十六进制
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getBalance","params":["0x7f5440ce55db40......d79defc87c7c"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0x21dfe1f5c5363780000"}

-------------------------------------------------------------------------------------

.. _getBlock:

:接口: `mh`_.getBlock

:描述: 获得联盟链某一区块信息。

:输入参数: 

- 块号，十六进制
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getBlock","params":["0x3"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"extraData":"0x18bcb765b41fdc06478c......8d8f09f17f63f1c803","hash":"0x558c038641ca32fe003d6......891571043b53665134cc28542952e7","miner":"0x44268c92ec660......ff1bfcbf9dfd14276c1","number":"0x3","parentHash":"0x2871f7a292b6b433183f2ee867......ca8bcb5b31eba01b054","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5......c001622fb5e363b421","stateRoot":"0x5e7b9dc44d4bbe962c1f9c......620ef1c484022a1a69e0a4b","timestamp":"0x5e85821c","transactions":[],"transactionsRoot":"0x56e81f171bcc5......e5b48e01b996cadc001622fb5e363b421"}}

-------------------------------------------------------------------------------------

.. _getBlockList:

:接口: `mh`_.getBlockList

:描述: 获取联盟链某一区间内的区块信息。

:输入参数: 

-  起始块号，十六进制
-  结束块号，十六进制
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getBlockList","params":["0x3","0x4"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"blockList":[{"extraData":"0x18bcb765b41fdc06478c9da......17f63f1c803","hash":"0x558c038641ca32fe003......43b53665134cc28542952e7","miner":"0x44268c92e......bfcbf9dfd14276c1","number":"0x3","parentHash":"0x2871f7a2......58ca8bcb5b31eba01b054","receiptsRoot":"0x56e81......b996cadc001622fb5e363b421","stateRoot":"0x5e7b9dc44......7d86620ef1c484022a1a69e0a4b","timestamp":"0x5e85821c","transactions":[],"transactionsRoot":"0x56e81f171bcc5......adc001622fb5e363b421"},{"extraData":"0x46c74da4d4d62e......8724fc009d3b07","hash":"0x42d510......b0f9bb05e2085d2c07ac15caf90","miner":"0xe569b8d860......66eb24a78de3fed","number":"0x4","parentHash":"0x558c038641ca32fe003d6d......134cc28542952e7","receiptsRoot":"0x56e81f171bcc55a6......dc001622fb5e363b421","stateRoot":"0x5e7b9dc4......f1c484022a1a69e0a4b","timestamp":"0x5e858226","transactions":[],"transactionsRoot":"0x56e81f171bcc......001622fb5e363b421"}],"endBlk":"0x4","startBlk":"0x3"}}

-------------------------------------------------------------------------------------

.. _getBlockNumber:

:接口: `mh`_.getBlockNumber

:描述: 获得当前联盟链的区块高度。

:输入参数：无

:返回:当前区块高度，十六进制
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getBlockNumber","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"0x33f"}

-------------------------------------------------------------------------------------

.. _getContractAddrList:

:接口: `mh`_.getContractAddrList

:描述: 

获取联盟链内所有已注册合约的地址列表，需要已部署的合约在dappbase中调用registerDapp方法后才能生效，具体方法请参见

:输入参数：无

:返回:合约地址列表，第一个是dappbase地址，之后是已注册合约地址
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getContractAddrList","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":["0x974c37d2b......285cf5126512a"]}

-------------------------------------------------------------------------------------

.. _getNonce:

:接口: `mh`_.getNonce

:描述: 获得对应账号在联盟链中的Nonce，这是调用联盟链DAPP合约的必要参数之一，每当联盟链交易发送后会自动加1

-  账号地址

:返回:该账号Nonce
 
示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getNonce","params":["0x7f5440ce55db......6d79defc87c7c"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":2}

-------------------------------------------------------------------------------------

.. _getReceiptByHash:

:接口: `mh`_.getReceiptByHash

:描述: 通过交易hash获取联盟链的tx执行结果

:输入参数：

-  账号地址

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getReceiptByHash","params":["0x8c6b2a2c1392839d04223bc3abf0b31e588ea3c7c70e4ebf62abee024a04f47f"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"contractAddress":"0x00000000000......0000000000000000","failed":false,"logs":[],"logsBloom":"0x0000000000000000000......000000000000000000000000000","queryInBlock":0,"result":"","transactionHash":"0x8c6b2a2c139......e588ea3c7c70e4ebf62abee024a04f47f"}}

-------------------------------------------------------------------------------------

.. _getReceiptByNonce:

:接口: `mh`_.getReceiptByNonce

:描述: 通过账号和Nonce获取联盟链的tx执行结果

:输入参数:

-  账号地址
-  Nonce

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getReceiptByNonce","params":["0x7f5440ce55d......6d79defc87c7c", 1]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"contractAddress":"0x00000000......0000000000000000","failed":false,"logs":[],"logsBloom":"0x000000000000000000000000000000000000......00000000000000","queryInBlock":0,"result":"","transactionHash":"0x8c6b2a2c1392839d04223b......7c70e4ebf62abee024a04f47f"}}

-------------------------------------------------------------------------------------

.. _getTransactionByHash:

:接口: `mh`_.getTransactionByHash

:描述: 通过交易HASH获取联盟链的交易信息

:输入参数:

-  交易哈希

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getTransactionByHash","params":["0x8c6b2a2c1392839d......4ebf62abee024a04f47f"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"blockHash":"0x8593bf7ca8fd20f755598c8b......7b6cfcc936e3d98e3dc1","blockNumber":"0x2d0","from":"0x7f5440ce55db40......79defc87c7c","gas":null,"gasPrice":null,"hash":"0x8c6b2a2c139283......4ebf62abee024a04f47f","input":"0x","nonce":"0x1","syscnt":"0x0","to":"0xc4c17b9e04d7......8309cd81f42b67","transactionIndex":"0x0","value":"0xde0b6b3a7640000","v":"0xef","r":"0x1ea4ea0cd20......6bdf87143292b15af1a","s":"0xd8b7411188aabc29e9bec5......5b7be580e17c435f10955569d80d","shardingFlag":"0x0"}}

-------------------------------------------------------------------------------------

.. _getTransactionByNonce:

:接口: `mh`_.getTransactionByNonce

:描述: 通过账号和Nonce获取联盟链的交易信息

:输入参数：

-  账号地址
-  Nonce

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getTransactionByNonce","params":["0x7f5440ce55db......defc87c7c", 1]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"blockHash":"0x8593bf7ca8fd20f75......7b6cfcc936e3d98e3dc1","blockNumber":"0x2d0","from":"0x7f5440ce55d......defc87c7c","gas":null,"gasPrice":null,"hash":"0x8c6b2a2c1......bee024a04f47f","input":"0x","nonce":"0x1","syscnt":"0x0","to":"0xc4c17b9e04......a2d98309cd81f42b67","transactionIndex":"0x0","value":"0xde0b6b3a7640000","v":"0xef","r":"0x1ea4ea0cd20658......b72b76f9a5342e6bdf87143292b15af1a","s":"0xd8b7411188aabc29e9bec5bb2......c435f10955569d80d","shardingFlag":"0x0"}}

-------------------------------------------------------------------------------------

.. _syncing:

:接口: `mh`_.syncing

:描述: 获取联盟链节点同步信息

:输入参数：无

:返回:

- 开始块，十六进制
- 当前块，十六进制
- 最高块，十六进制

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_syncing","params":[]}'http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result": {startingBlock: '0x384',currentBlock: '0x386',highestBlock: '0x454'}}
    // Or when not syncing
    {"jsonrpc":"2.0","id":0,"result":false}

-------------------------------------------------------------------------------------

.. _getBlockThreshold:

:接口: `mh`_.getBlockThreshold

:描述: 获取联盟链下次续费块高度

:输入参数: 无

:返回:

- 当前块号
- 下次续费高度

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"mh_getBlockThreshold","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"Current":972,"Threshold":200000}}

|br|
|br|    

txpool
:::::::::::::::::::::::

content_： 获得联盟链交易池交易的详细信息

inspect_： 获得联盟链交易池交易的简要信息

status_： 获得联盟链交易池交易的交易数量

|br|
|br|

.. _content:

:接口: `txpool`_.content

:描述: 获得联盟链交易池交易的详细信息

:输入参数: 无

:返回:

- pending：待打包
- queued：队列中

示例: 
::
   // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"txpool_content","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"pending":{"0x7f5440ce55d......B06d79DEfC87C7c":{"2":{"blockHash":"0x000000000000000000000......0000000000000000000000","blockNumber":null,"from":"0x7f5440ce......ec0b06d79defc87c7c","gas":null,"gasPrice":null,"hash":"0xff5d8......8ac013c9e491","input":"0x","nonce":"0x2","syscnt":"0x0","to":"0xc4c17b9e0......81f42b67","transactionIndex":"0x0","value":"0xde0b6b3a7640000","v":"0xef","r":"0x8494dbb7f3af......78f597406de","s":"0x7d25dd2abc4dafe......5e724c9e66cc8c72e901421ba607c5c9606","shardingFlag":"0x0"}}},"queued":{}}}

-------------------------------------------------------------------------------------

.. _inspect:

:接口: `txpool`_.inspect

:描述: 获得联盟链交易池交易的简要信息

:输入参数: 无

:返回: 

- pending：待打包
- queued：队列中

示例: 
::
     // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"txpool_inspect","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"pending":{"0x7f5440ce55db4......B06d79DEfC87C7c":{"3":"0xC4c17b9e04d75......Cd81F42B67: 1000000000000000000 sha + 0 × 0 gas"}},"queued":{}}}

-------------------------------------------------------------------------------------

.. _status:

:接口: `txpool`_.status

:描述: 获得联盟链交易池交易的交易数量

:输入参数：无

:返回：

- pending：待打包，十六进制
- queued：队列中，十六进制

示例: 
::
    // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"txpool_status","params":[]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":{"pending":"0x1","queued":"0x0"}}

|br|
|br|    

debug
:::::::::::::::::::::::

traceTransaction_： 获得交易在EVM执行期间创建的结构化日志

|br|
|br|

.. _traceTransaction:

:接口: `debug`_.traceTransaction

:描述: 返回交易在EVM执行期间创建的结构化日志，并将它们作为JSON对象返回

:输入参数:

- 交易哈希

示例: 
::
     // Request
    curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"debug_traceTransaction","params":["0x1a0da52d43d4222......4100f7357f0a0128340", "callTracer"]}' http://127.0.0.1:8546/rpc

    // Result
    {"jsonrpc":"2.0","id":0,"result":"{\"type\":\"CALL\",\"from\":\"0xc2a0423fac6......9bd8560288edc94c00ee\",\"to\":\"0xe5d7da7......94c43a7e16575f18e4b746\",\"value\":\"0x0\",\"gas\":\"0x337be36\",\"gasUsed\":\"0x0\",\"input\":\"0x12df941200000000000000000000......00000000000000000000000000000005e562360\",\"output\":\"0x\",\"time\":\"0s\",\"calls\":[{\"type\":\"CALL\",\"from\":\"0xe5d7da7a37466321......7e16575f18e4b746\",\"to\":\"0xf180041c895a6aa......ea3e20c2cbe\",\"value\":\"0xde0b6b3a7640000\",\"input\":\"0x\"}]}"}

