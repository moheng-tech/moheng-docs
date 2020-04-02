联盟链命令行介绍
-----------------------------

Console是指联盟链开发中的命令行模式，用于开发人员直接通过命令行方式对链上的数据进行调试。当前 :doc:`SSN <../Definitions>` 和SSB都有命令行模式。命令行模式适用于联盟链部署和debug链上数据，适合开发人员使用。

另外两种 :doc:`SDK <./Sdk>` 和 :doc:`RPC <./Rpc>` 的开发模式，将在本章其他节介绍。

启动Console的方式
>>>>>>>>>>>>>>>>>>>>>>>>>>

**启动SSB console的方式**

**前提：** 已经启动SSB
然后在同一台服务器中输入
::
    super-solid-base attach 

**启动SSN console的方式**

**前提：** 已经启动SSN
然后在同一台服务器中输入
::
    super-solid-node attach 


SSN console命令汇总
>>>>>>>>>>>>>>>>>>>>>>>>>>
联盟链主要是在SSN上进行命令行调试，以下是当前常用的命令行范例。

本节将按照前缀进行分类。

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



.. |br| raw:: html

    <br/>

|br|
|br|

.. _getSSNId:

:接口: `mh`_.getSSNId

:描述: 获取SSN节点id 

:输入参数: 无

:返回：

- SSN 地址
 
示例: 
::
    > mh.getSSNId()

    "0x76449055dc3cf91090c11f7c544b60363bf896cb"

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
    > mh.sendRawTransaction("0xf86a33808504a817c8008094a......3364503ba2b67599ab218117b3182a30")

    "0xa629303563c97821fca......7e2963cb06692704d84fbc05946e5576a5"

-------------------------------------------------------------------------------------

.. _accounts:

:接口: `mh`_.accounts

:描述: 获取SSN节点已创建地址列表 

:输入参数: 无

:返回：

- SSN 地址列表集
 
示例: 
::
    > mh.accounts

    ["0x76449055dc3cf9109.......c544b60363bf896cb", "0x9441c6707a1......119f27497c8227d"]

-------------------------------------------------------------------------------------

.. _sign:

:接口: `mh`_.sign

:描述: 

以特定前缀签名,通过向消息添加前缀，可以将计算得到的签名识别为moheng特定的签名。这可以防止恶意的DApp对任意数据(如事务)进行签名，并使用签名冒充受害者。注意要签名的地址必须解锁。

:输入参数: 

- 加签账号地址
- 加签明文数据

:返回：

- 签名结果
 
示例: 
::
    > mh.sign("0x69f3d468cbec......b203668518dcd18d11b16", "0x53220b11be2c92dbb4......73f93eb95385e92a9ed29bf9d07d79f534b")

    "0x8f647ae2563179eed1ceed810300......1784ba8675794a17639e5ae6b52f88d1b"

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

:返回：

- raw: 交易加签结果
 
示例: 
::
    > mh.signTransaction({from:mh.accounts[1], to: "0xaa4c98c7efb24......b437cd761df4e648c", value:'0x5f484e5a'})

    {
        raw: "0xf8658080808094aa4c98c7efb2......5b7b2a7b6d3e37b29e5eb548aa073e0436ca1318f7f9",
        tx: {
            ......
        }
    }

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

:返回：

- 交易hash
 
示例: 
::
   > mh.sendTransaction({from:mh.accounts[1], to: "0xaa4c98c7efb244f4......7cd761df4e648c", value:'0x0'})

    "0x06e6153b5878420cedb3850fe3b486......add715edfa24b606b6355b802"

-------------------------------------------------------------------------------------

.. _anyCall:

:接口: `mh`_.anyCall

:描述: 获取SSN合约函数的返回值，调用此接口前必须将合约注册入dappbase

:输入参数: 

-  Sender：查询账号
-  DappAddr:联盟链业务逻辑地址
-  Params： 第一个参数是调用的方法，之后是方法传入参数

:返回：

- 函数返回值
 
示例: 
::
   > mh.anyCall({Sender: "0xc2a0423fac6d......bd8560288edc94c00ee", DappAddr:"0xe5d7da7a37......4c43a7e16575f18e4b746", Params:['getCurNodeList']})

    "{\"nodeList\":[\"0x76449055dc3cf......b60363bf896cb\",\"0xc2a0423fac6d1ae......0288edc94c00ee\"]}"

-------------------------------------------------------------------------------------

.. _getBalance:

:接口: `mh`_.getBalance

:描述: 获得对应账号在联盟链中的货币余额。

:输入参数: 

-  账号地址

:返回：

- 账号余额，精度18
 
示例: 
::
  > mh.getBalance('0xf180041c895a6aa......277cea3e20c2cbe')

    11000000000000000000

-------------------------------------------------------------------------------------

.. _getBlock:

:接口: `mh`_.getBlock

:描述: 获得联盟链某一区块信息。

:输入参数: 

- 块号
 
示例: 
::
    > mh.getBlock(37)

    {
      extraData: "0xa77c68b8d817377d61c23bcea32c57eab4db5......256ff80301401c1d7423d512504cc588ab68dd36f90fcd1642b1dfb0a79564f44432311a5fd00ef252dbe79d002929aa42ddaf1650a7b8f18c59edfaa9189d025923f1617a510209313538323638373437034062e8e2de7b1d5af97dc66e51fc93102ebdb5768431e36f65d8ff02af61c7016ba97b07d36cbd22d0ce8da0d7c0c8a13aa6d9fd42ab274d588610442a62d2b00e",
      hash: "0xfe8f215cb8b70d43d50c185......de2914c443ac064d436807e39a7",
      miner: "0xc2a0423fac6d......bd8560288edc94c00ee",
      number: "0x25",
      parentHash: "0xc881942213......f9598798343f0a1793006eab6ce84272da5428ce",
      receiptsRoot: "0xa07cd7906ea8b......aa57a7cd068c6f2acd7a165cf599537022b9d8fc4",
      stateRoot: "0xd6f76d1619ae985d......d7c530569d864323d16d248b8cd3f4581e40e",
      timestamp: "0x5e55e4ee",
      transactions: ["0x36249ce5e68.......19c5d3f4e7e63d805416d5c038a611b111cd9a43a9", "0xaaf14ac0152fb12f71cce01fb8.......0c5fc4d0cbb27cc64f3f78b2720bca6b"],
      transactionsRoot: "0xb7ece0cd0fcc......23062dc25fc66529fa5093ae30859f91a11298676bc0f0"
    }

-------------------------------------------------------------------------------------

.. _getBlockList:

:接口: `mh`_.getBlockList

:描述: 获取联盟链某一区间内的区块信息。

:输入参数: 

-  起始块号
-  结束块号
 
示例: 
::
    > mh.getBlockList(36, 37)
    {
      blockList: [{
          extraData: "0xf0c9626d45789730f1fcb773907f6bae0d4......1180f4c39f7fd3daf67ee61d79f8e239c3b14ed160f8a606",
          hash: "0xc881942213aad1bad5403c47f95......93006eab6ce84272da5428ce",
          miner: "0xc2a0423fac6d1aee9......8560288edc94c00ee",
          number: "0x24",
          parentHash: "0xc5dd98df039f1d5......1c197b666c1db038da2f225bd8a9772de966d4",
          receiptsRoot: "0x56e81f171bcc55a6......2c0f86e5b48e01b996cadc001622fb5e363b421",
          stateRoot: "0x90766f90ee1d7dfc029......4cfcb94d6f370b78ff522b3066235325",
          timestamp: "0x5e55e4e4",
          transactions: [],
          transactionsRoot: "0x56e81f171bcc55a......f86e5b48e01b996cadc001622fb5e363b421"
      }, {
          extraData: "0xa77c68b8d817377d61c23.......442a62d2b00e",
          hash: "0xfe8f215cb8b70d43d50c185fb0b7......5de2914c443ac064d436807e39a7",
          miner: "0xc2a0423fac6d1aee......d8560288edc94c00ee",
          number: "0x25",
          parentHash: "0xc881942213aa......9598798343f0a1793006eab6ce84272da5428ce",
          receiptsRoot: "0xa07cd7906ea8b......a57a7cd068c6f2acd7a165cf599537022b9d8fc4",
          stateRoot: "0xd6f76d1619ae985......583dd7c530569d864323d16d248b8cd3f4581e40e",
          timestamp: "0x5e55e4ee",
          transactions: ["0x36249ce5e68......719c5d3f4e7e63d805416d5c038a611b111cd9a43a9", "0xaaf14ac0152fb12f71cce01fb8e244cc......d0cbb27cc64f3f78b2720bca6b"],
          transactionsRoot: "0xb7ece0cd0fcc52......62dc25fc66529fa5093ae30859f91a11298676bc0f0"
      }],
      endBlk: "0x25",
      startBlk: "0x24"
    }

-------------------------------------------------------------------------------------

.. _getBlockNumber:

:接口: `mh`_.getBlockNumber

:描述: 获得当前联盟链的区块高度。

:输入参数：无

:返回：当前区块高度
 
示例: 
::
> mh.getBlockNumber()

2245

-------------------------------------------------------------------------------------

.. _getContractAddrList:

:接口: `mh`_.getContractAddrList

:描述: 获取联盟链内所有已注册合约的地址列表，需要已部署的合约在dappbase中调用registerDapp方法后才能生效，具体方法请参见

:输入参数：无

:返回：合约地址列表，第一个是dappbase地址，之后是已注册合约地址
 
示例: 
::
    > mh.getContractAddrList()

    ["0xe5d7da7a3746......7e16575f18e4b746"]

-------------------------------------------------------------------------------------

.. _getNonce:

:接口: `mh`_.getNonce

:描述: 获得对应账号在联盟链中的Nonce，这是调用联盟链DAPP合约的必要参数之一，每当联盟链交易发送后会自动加1

-  账号地址

:返回：该账号Nonce
 
示例: 
::
    > mh.getNonce('0xf180041c895......277cea3e20c2cbe')

    1

-------------------------------------------------------------------------------------

.. _getReceiptByHash:

:接口: `mh`_.getReceiptByHash

:描述: 通过交易hash获取联盟链的tx执行结果

:输入参数：

-  账号地址

示例: 
::
    > mh.getReceiptByHash('0x9cf04d4f00998ba09c2c6......85b173a406baedde414f08d88df5c992034')
    {
      contractAddress: "0x000000000000......000000000000000000",
      failed: false,
      logs: [],
      logsBloom: "0x00000000000000000......00000000000000000000000000000000000000",
      queryInBlock: 0,
      result: "",
      transactionHash: "0x9cf04d4f00998ba09c2c6......73a406baedde414f08d88df5c992034"
    }

-------------------------------------------------------------------------------------

.. _getReceiptByNonce:

:接口: `mh`_.getReceiptByNonce

:描述: 通过账号和Nonce获取联盟链的tx执行结果

:输入参数：

-  账号地址
-  Nonce

示例: 
::
    > mh.getReceiptByNonce('0xf180041c895a6aA8b2......0A277cEA3e20C2CBE',2)
    {
      contractAddress: "0x000000000000000......0000000000000000",
      failed: false,
      logs: [],
      logsBloom: "0x000000000000000.......00000000000000000",
      queryInBlock: 0,
      result: "",
      transactionHash: "0x9cf04d4f00998ba0.......85b173a406baedde414f08d88df5c992034"
    }

-------------------------------------------------------------------------------------

.. _getTransactionByHash:

:接口: `mh`_.getTransactionByHash

:描述: 通过交易HASH获取联盟链的交易信息

:输入参数：

-  交易哈希

示例: 
::
    > mh.getTransactionByHash('0x9cf04d4f00998ba09c......06baedde414f08d88df5c992034')
    {
      blockHash: "0xdaf05facc57d218528e6c......51eeed54a9fae9e8be4353c23c58ec6",
      blockNumber: "0x910",
      from: "0xf180041c895a6aa8b2......a277cea3e20c2cbe",
      gas: null,
      gasPrice: null,
      hash: "0x9cf04d4f00998ba09c......5b173a406baedde414f08d88df5c992034",
      input: "0x3876c45f5b6f81......cf5ebcb075e400a571e",
      nonce: "0x2",
      r: "0x5c1aeea440bdf02cdf646......5ed29d42a41c59b299262ffb23a4a5e7",
      s: "0x7dac198898963a33a69af......aab94328c0c03215467032055eb2f26b716d",
      shardingFlag: "0x2",
      syscnt: "0x0",
      to: "0xaa4c98c7efb24......bb437cd761df4e648c",
      transactionIndex: "0x0",
      v: "0xf0",
      value: "0xde0b6b3a7640000"
    }

-------------------------------------------------------------------------------------

.. _getTransactionByNonce:

:接口: `mh`_.getTransactionByNonce

:描述: 通过账号和Nonce获取联盟链的交易信息

:输入参数：

-  账号地址
-  Nonce

示例: 
::
    > mh.getTransactionByNonce('0xf180041c895a6aA......20C2CBE',2)
    {
      blockHash: "0xdaf05facc57d218528e6c73df......4a9fae9e8be4353c23c58ec6",
      blockNumber: "0x910",
      from: "0xf180041c895a......500a277cea3e20c2cbe",
      gas: null,
      gasPrice: null,
      hash: "0x9cf04d4f00998ba0......4fd85b173a406baedde414f08d88df5c992034",
      input: "0x3876c45f5b6f81......cf5ebcb075e400a571e",
      nonce: "0x2",
      r: "0x5c1aeea440bdf02cdf6.......3c5ed29d42a41c59b299262ffb23a4a5e7",
      s: "0x7dac198898963a33a69af......b94328c0c03215467032055eb2f26b716d",
      shardingFlag: "0x2",
      syscnt: "0x0",
      to: "0xaa4c98c7efb24......b437cd761df4e648c",
      transactionIndex: "0x0",
      v: "0xf0",
      value: "0xde0b6b3a7640000"
    }

-------------------------------------------------------------------------------------

.. _syncing:

:接口: `mh`_.syncing

:描述: 获取联盟链节点同步信息

:输入参数：无

:返回：

- 开始块
- 当前块
- 最高块

示例: 
::
    > mh.syncing()
    {
        startingBlock: '0x384',
        currentBlock: '0x386',
        highestBlock: '0x454'
    }

-------------------------------------------------------------------------------------

.. _getBlockThreshold:

:接口: `mh`_.getBlockThreshold

:描述: 获取联盟链下次续费块高度

:输入参数：无

:返回：

- 当前块号
- 下次续费高度

示例: 
::
    > mh.getBlockThreshold()
    {
      Current: 312,//当前块号
      Threshold: 200000//下次续费块高度
    }

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

:输入参数：无

:返回：

- pending：待打包
- queued：队列中

示例: 
::
    > txpool.content()
    {
      pending: {},
      queued: {
        0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE: {
          3: {
            blockHash: "0x000000000000000000......0000000000000000000000000000000000000000",
            blockNumber: null,
            from: "0xf180041c895a......38500a277cea3e20c2cbe",
            gas: null,
            gasPrice: null,
            hash: "0x58eb432ff88f7666......5d92f0a577e6e769712f1a2356fc1c25bde9110",
            input: "0x3876c45f5b6f81......5ebcb075e400a571e",
            nonce: "0x3",
            r: "0x3c79d4f4b1336565db6234af......2c1aee1a3107bf92e97361c0b3",
            s: "0x7f582d62b3e99f5dad7d34dc......37f70811aa90c645a1fdd433f349338",
            shardingFlag: "0x2",
            syscnt: "0x0",
            to: "0xaa4c98c7efb244f41......7cd761df4e648c",
            transactionIndex: "0x0",
            v: "0xf0",
            value: "0x0"
          }
        }
      }
    }

-------------------------------------------------------------------------------------

.. _inspect:

:接口: `txpool`_.inspect

:描述: 获得联盟链交易池交易的简要信息

:输入参数：无

:返回：

- pending：待打包
- queued：队列中

示例: 
::
     > txpool.inspect()
    {
      pending: {},
      queued: {
        0xf180041.......00A277cEA3e20C2CBE: {
          3: "0xAA4c98C7efb24......3bB437CD761Df4e648C: 0 sha + 0 × 20000000000 gas"
        }
      }
    }

-------------------------------------------------------------------------------------

.. _status:

:接口: `txpool`_.status

:描述: 获得联盟链交易池交易的交易数量

:输入参数：无

:返回：

- pending：待打包
- queued：队列中

示例: 
::
     > txpool.status()
    {
      pending: 0,
      queued: 1
    }

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

:输入参数：

- 交易哈希

示例: 
::
     >debug.traceTransaction("1a0da52d43d42223c......6aba732a74100f7357f0a0128340", "callTracer")

    "{\"type\":\"CALL\",\"from\":\"0xc2a0423fac6d1ae......bd8560288edc94c00ee\",\"to\":\"0xe5d7da7a374663218294c43a7e16575f18e4b746\",\"value\":\"0x0\",\"gas\":\"0x337be36\",\"gasUsed\":\"0x0\",\"input\":\"0x12df94120000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000f180041c895a6aa8b2f38500a277cea3e20c2cbe00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000de0b6b3a76400000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000005e562360\",\"output\":\"0x\",\"time\":\"0s\",......5a6aa8b2f38500a277cea3e20c2cbe\",\"value\":\"0xde0b6b3a7640000\",\"input\":\"0x\"}]}"

-------------------------------------------------------------------------------------

|br|
|br|    

personal
:::::::::::::::::::::::

newAccount_： 创建新的账号

listAccounts_: 获取下SSN的账号列表

unlockAccount_：解锁指定账号

lockAccount_：锁定账号

ecRecover_：地址验签

|br|
|br|  


.. _newAccount:

:接口: `personal`_.newAccount

:描述: 创建新的账号

:输入参数：

- 账号密码

示例: 
::
     >personal.newAccount('123456')

    "0x69f3d468cbec......203668518dcd18d11b16"

-------------------------------------------------------------------------------------

.. _listAccounts:

:接口: `personal`_.listAccounts

:描述: 获取下SSN的账号列表

:输入参数：无

示例: 
::
     > personal.listAccounts
    ["0x76449055dc3cf.......0363bf896cb", "0x9441c6707a1ef......634a119f27497c8227d"]

-------------------------------------------------------------------------------------

.. _unlockAccount:

:接口: `personal`_.unlockAccount

:描述: 解锁指定账号

- 解锁账号地址
- 解锁账号密码

:输入参数：
- 账号密码

示例: 
::
     > personal.unlockAccount(mh.accounts[3], '123456')
    true

-------------------------------------------------------------------------------------

.. _lockAccount:

:接口: `personal`_.lockAccount

:描述: 锁定账号

- 解锁账号地址

:输入参数：

- 账号密码

示例: 
::
     > personal.lockAccount(mh.accounts[3])
    true

-------------------------------------------------------------------------------------

.. _ecRecover:

:接口: `personal`_.ecRecover

:描述: ecRecover返回用于创建签名的帐户的地址。注意，此函数与mh.sign和personal.sign兼容。

:输入参数：

- 加签明文数据
- 加签密文数据

:返回：

- 账户地址

示例: 
::
 > personal.ecRecover("0x53220b11be2c92dbb40be6159......92a9ed29bf9d07d79f534b", "0x8f647ae2563......63a203686c0b513822a9935b2c8c51784ba8675794a17639e5ae6b52f88d1b")

    "0x69f3d468cbec148984a......518dcd18d11b16"

