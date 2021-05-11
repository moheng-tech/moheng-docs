联盟链SDK-NODE介绍
-----------------------------

SDK是作为专为客户端使用的软件开发包，极大方便了开发人员在客户端直接调用区块链相关接口。当前SDK可提供nodejs版本。SDK可同时连接 :doc:`SSN <../Definitions>` 和SSB。

另外两种 :doc:`Console <./Console>` 和 :doc:`RPC <./Rpc>` 的开发模式，将在本章其他节介绍。

SDK的1.0.8及其之后的版本已支持国密加密算法和国际加密算法两个版本，以下接口中会对两种算法分别进行详述，若接口中没区分两种算法，则两种算法使用方式一致，注意交易时的钱包地址和钱包私钥区分是否国密版本，如使用国密算法的交易，请使用国密注册账户的钱包地址和钱包私钥。

SDK的安装
>>>>>>>>>>>>>>>>>>>>>>>>>>

可使用如下命令安装联盟链nodejs版本的SDK
::
    npm install super-solid-link

.. |br| raw:: html

    <br/>

|br|

nodejs版SDK详述
>>>>>>>>>>>>>>>>>>>>>>>>>>

异常处理
::::::::::::::::::::

通常，采用如下方式进行异常的捕获
::
    var ssb = require("super-solid-link").ssb;

    try{
            var ssbobj = new ssb("http://127.0.0.1:8545");
            var blockNumber = ssbobj.getBlockNumber();
            console.log(blockNumber);
    }catch (e){
            console.log(e);
    }

账户注册
:::::::::::::::::::::
    
国际加密算法
'''''''''''''''''''''''''''

参数:
::
    pwd：账户密码

代码:
::

    var account = require("super-solid-link").account;
    var wallet = account.register(pwd);

返回:
::

    wallet：
    { address: '账户地址....',
      privateKey: '私钥....',
      keyStore: 'keyStore内容...' 
    }
    
国密加密算法
'''''''''''''''''''''''''''

参数:
::
    pwd：账户密码

代码:
::

    var account = require("super-solid-link").account;
    var wallet = account.registerSM(pwd);

返回:
::

    wallet：
    { address: '账户地址....',
      privateKey: '私钥....',
      keyStore: 'keyStore内容...' 
    }
  
账户登录
:::::::::::::::::::::

国际加密算法
'''''''''''''''''''''''''''

参数:
::

    addr：账户地址
    pwd：账户密码
    keyStore：keyStore字符串

代码:
::

    var account = require("super-solid-link").account;
    var privateKey = account.login(addr, pwd, keyStore);

返回:
::

    privateKey： 账户私钥

国密加密算法
'''''''''''''''''''''''''''

参数:
::

    addr：国密账户地址
    pwd：国密账户密码
    keyStore：国密keyStore字符串

代码:
::

    var account = require("super-solid-link").account;
    var privateKey = account.loginSM(addr, pwd, keyStore);

返回:
::

    privateKey： 账户私钥


----------------------------------------------------------------------------------------------

SSB模块接口
>>>>>>>>>>>>>>>>>>>>>>>>>

**SSB只介绍部署时需要用到的接口** 


实例化SSB对象
:::::::::::::::::::::::::
在使用接口前，需要打开一个节点的 :doc:`RPC <./Rpc>` 并允许外部访问。

参数:
::
    ssbAddress：基础链访问地址 http://127.0.0.1:8545
    
代码:
::

    var ssb = require("super-solid-link").ssb;
    var ssbobj = new ssb(ssbAddress);

获取基础链区块高度
:::::::::::::::::::::::::::::::::::::::::::


代码:
::
    var blockNumber = ssbobj.getBlockNumber();

返回:
::
    blockNumber：基础链区块高度

-------------------------------------------------------------------------------------------

SSN模块接口
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


实例化ssn对象
:::::::::::::::::::::::::::::::::

国际加密算法
'''''''''''''''''''''''''''

参数:
::
    ssnAddress：ssn访问地址 //http://127.0.0.1:8546


代码:
::
    var ssn = require("super-solid-link").ssn;
    var ssnobj = new ssn(ssnAddress);
    
国密加密算法
'''''''''''''''''''''''''''

参数:
::
    ssnAddress：ssn访问地址 //http://127.0.0.1:8546


代码:
::
    var ssn = require("super-solid-link").ssn;
    var ssnobj = new ssn(ssnAddress,"sm");

获取联盟链ssnId
:::::::::::::::::::::::::


代码:
::
    ssnobj.getSsnId().then((sscId) => {
        console.log(sscId);
    });

返回:
::
    sscId：联盟链ssnId
	
	
获取下次续费块高度
:::::::::::::::::::::::::::::


代码:
::
    ssnobj.getBlockThreshold().then((data) => {
        console.log(data);
    });

返回:
::
    data.Current：当前块高度
	data.Threshold：下次续费块高度
	
获取联盟链区块高度
::::::::::::::::::::::::::::::::::::::::::


代码:
::
    ssnobj.getBlockNumber().then((blockNumber) => {
        console.log(blockNumber);
    });

返回:
::
    blockNumber：联盟链区块高度
    
获取某一区间内的多个区块信息
:::::::::::::::::::::::::::::::::::::::::::::::::

参数:
::
    start：开始高度
    end：结束高度

代码:
::
    ssnobj.getBlockList(start, end).then((blockListInfo) => {
        console.log(blockListInfo);
    });

返回:
::
    blockListInfo：区块信息List
    
获取联盟链某一区块信息
::::::::::::::::::::::::::::::::::::::::::

参数:
::
    blockNumber：区块高度

代码:
::
    ssnobj.getBlock(blockNumber).then((blockInfo) => {
        console.log(blockInfo);
    });

返回:
::
    blockInfo：某一区块信息
    
通过交易HASH获取联盟链的交易信息
::::::::::::::::::::::::::::::::::::::::::::::::::::::::


参数:
::
    transactionHash：交易hash

代码:
::
    ssnobj.getTransactionByHash(transactionHash).then((transactionInfo) => {
        console.log(transactionInfo);
    });

返回:
::
    transactionInfo：交易详情
	
通过交易hash获取联盟链的tx执行结果
::::::::::::::::::::::::::::::::::::::::::


参数:
::
    transactionHash：交易hash

代码:
::
    ssnobj.getTransactionReceiptByHash(transactionHash).then((result) => {
        console.log(result);
    });

返回:
::
    result：执行结果
	
获取联盟链已注册合约列表
::::::::::::::::::::::::::::::::::


代码:
::
    ssnobj.getContractAddrList().then((result) => {
        console.log(result);
    });

返回:
::
    result：合约列表
    
获取联盟链账户余额
::::::::::::::::::::::::::::::::::


参数:
::
    addr：账户地址

代码:
::
    ssnobj.getBalance(addr).then((balance) => {
        console.log(balance);
    });

返回:
::
    data：联盟链账户余额（erc20最小单位）
    
    
获取Nonce
:::::::::::::::::::::::::


参数:
::
    addr：账户钱包地址

代码:
::
    ssnobj.getNonce(addr).then((nonce) => {
        console.log(nonce);
    });;

返回:
::
    nonce：得到的nonce
    
获取联盟链详细信息
:::::::::::::::::::::::::

代码:
::
    ssnobj.getAppChainInfo().then((appChainInfo) => {
        console.log(appChainInfo);
    });

返回:
::
    appChainInfo：联盟链信息

   
调用联盟链合约
::::::::::::::::::::::::

参数:
::
    contractAddress：dapp合约地址
    param：例如合约中存在一个无参的方法getDechatInfo，则传入["getDechatInfo"];
             存在一个有参的方法getTopicList(uint pageNum, uint pageSize), 则传入["getTopicList", 0, 20]

代码:
::
    ssnobj.callContract(contractAddress, param).then((data) => {
        console.log(data);
    });

返回:
::
    data：调用合约返回信息
    
调用墨珩联盟链合约
::::::::::::::::::::::::

参数:
::
    contractAddress: dapp合约地址
    FuncName: 例如合约中存在一个方法名，如issue;
    method: 方法，例如issue(address,uint256)
    paramTypes: 参数类型数组 ['address','uint256']
    paramValues: 参数值数组 ['0x.....',10000]

代码:
::
    ssnobj.mhCallContract(contractAddress, FuncName, method, paramTypes, paramValues).then((data) => {
        console.log(data);
    });

返回:
::
    data：调用合约返回信息
	    
获取交易Data
:::::::::::::::::::::::::

参数:
::
    method：方法 例 "issue(address,uint256)"
    paramTypes：paramTypes 参数类型数组 例['address','uint256']
    paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）

代码:
::
    var data = ssnobj.getData(method,paramTypes,paramValues);

返回:
::
    data：data字符串


联盟链加签交易
:::::::::::::::::::::::::

参数:
::
    from：发送方的钱包地址
    contractAddress：联盟链合约地址
    amount：交易金额
    method：方法 例 "issue(address,uint256)"
    paramTypes：paramTypes 参数类型数组 例['address','uint256']
    paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
    privateKey：发送方钱包私钥
    nonce：发送方账户nonce(非必填)

代码:
::
    ssnobj.sendRawTransaction(from, contractAddress, amount, method, paramTypes, paramValues, privateKey, nonce).then((hash) => {
        console.log(hash);
    });

返回:
::
    hash：交易hash
    
加签交易（转账）
::::::::::::::::::::::::

参数:
::
    from：交易发起人
    to：交易接收人
    amount：交易金额
    strData：交易备注
    privateKey：交易发起人私钥
    nonce：发起人账户nonce(非必填)

代码:
::
    ssnobj.sendRawTransactionPrivate(from, to, amount, strData, privateKey, nonce).then((hash) => {
        console.log(data);
    });

返回:
::
    hash：交易hash
	
获取本地加签交易
::::::::::::::::::::::::

参数:
::
    from：发送方的钱包地址
    contractAddress：联盟链合约地址
    amount：交易金额
    method：方法 例 "issue(address,uint256)"
    paramTypes：paramTypes 参数类型数组 例['address','uint256']
    paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
    privateKey：发送方钱包私钥

代码:
::
    ssnobj.getSignedTx(from, contractAddress, amount, method, paramTypes, paramValues, privateKey).then((signedTx) => {
        console.log(signedTx);
    });

返回:
::
    signedTx：交易加签后交易体
	
发送已加签好的交易
::::::::::::::::::::::::

参数:
::
    signTx：交易加签后交易体

代码:
::
    ssnobj.sendSignTransaction(signTx).then((hash) => {
        console.log(hash);
    });

返回:
::
    hash：交易hash





