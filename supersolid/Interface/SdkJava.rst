联盟链SDK-JAVA介绍
-----------------------------

SDK是作为专为客户端使用的软件开发包，极大方便了开发人员在客户端直接调用区块链相关接口。当前SDK是java版本。SDK可连接 :doc:`SSN <../Definitions>` 。

另外两种 :doc:`Console <./Console>` 和 :doc:`RPC <./Rpc>` 的开发模式，将在本章其他节介绍。

SDK的1.0.1及其之后的版本已支持国密加密算法和国际加密算法两个版本，以下接口中会对两种算法分别进行详述，若接口中没区分两种算法，则两种算法使用方式一致，注意交易时的钱包地址和钱包私钥区分是否国密版本，如使用国密算法的交易，请使用国密注册账户的钱包地址和钱包私钥。

SDK的安装
>>>>>>>>>>>>>>>>>>>>>>>>>>

maven 资源包
::
    <dependency>
      <groupId>tech.moheng</groupId>
      <artifactId>super-solid-link</artifactId>
      <version>1.0.1</version>
    </dependency>

.. |br| raw:: html

    <br/>

|br|

JAVA版SDK详述
>>>>>>>>>>>>>>>>>>>>>>>>>>

异常处理
::::::::::::::::::::

通常，采用如下方式进行异常的捕获
::

    try {
        SSN ssn = new SSN("http://127.0.0.1:8546");
        String ssnId = ssn.getSsnId();
        System.out.println(ssnId);
    } catch (Exception e) {
        // TODO: handle exception
    }

账户注册
:::::::::::::::::::::
    
国际加密算法
'''''''''''''''''''''''''''

参数:
::
    password：账户密码

代码:
::

    Account account = new Account();
    JSONObject accountRes = account.register(password);

返回:
::

    accountRes：
    { address: '账户地址....',
      privateKey: '私钥....',
      keyStore: 'keyStore内容...' 
    }
    
国密加密算法
'''''''''''''''''''''''''''

参数:
::
    password：账户密码

代码:
::

    Account account = new Account();
    JSONObject accountRes = account.registerSM(password);

返回:
::

    accountRes：
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

    password：账户密码
    keyStore：keyStore字符串

代码:
::

    Account account = new Account();
    String privateKey = account.login(password, keyStore);

返回:
::

    privateKey： 账户私钥

国密加密算法
'''''''''''''''''''''''''''

参数:
::

    password：国密账户密码
    keyStore：国密keyStore字符串

代码:
::

    Account account = new Account();
    String privateKey = account.loginSM(password, keyStore);

返回:
::

    privateKey： 账户私钥


----------------------------------------------------------------------------------------------

SSN模块接口
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


实例化ssn对象
:::::::::::::::::::::::::::::::::

国际加密算法
'''''''''''''''''''''''''''

参数:
::
    ssnAddress: ssn访问地址 //http://127.0.0.1:8546


代码:
::
    String ssnAddress = "http://127.0.0.1:8546";
    SSN ssn = new SSN(ssnAddress);
    
国密加密算法
'''''''''''''''''''''''''''

参数:
::
    ssnAddress：ssn访问地址 //http://127.0.0.1:8546


代码:
::
    String ssnAddress = "http://127.0.0.1:8546";
    SSN ssn = new SSN(ssnAddress,"sm");

获取联盟链ssnId
:::::::::::::::::::::::::


代码:
::
    String ssnId = ssn.getSsnId();

返回:
::
    ssnId：联盟链ssnId
	
	
获取下次续费块高度
:::::::::::::::::::::::::::::


代码:
::
    JSONObject blockThreshold = ssn.getBlockThreshold();

返回:
::
    blockThreshold.Current：当前块高度
	blockThreshold.Threshold：下次续费块高度
	
获取联盟链区块高度
::::::::::::::::::::::::::::::::::::::::::


代码:
::
    BigDecimal blockNumber = ssn.getBlockNumber();

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
	int start = 10;
	int end = 12;
    JSONObject blockList = ssn.getBlockList(start, end);

返回:
::
    blockList：区块信息List
    
获取联盟链某一区块信息
::::::::::::::::::::::::::::::::::::::::::

参数:
::
    blockNumber：区块高度

代码:
::
    int blockNumber = 10;
    JSONObject blockInfo = ssn.getBlock(blockNumber);

返回:
::
    blockInfo：某一区块信息
    
通过交易HASH获取联盟链的交易信息
::::::::::::::::::::::::::::::::::::::::::::::::::::::::


参数:
::
    txHash：交易hash

代码:
::
    String txHash = "0x0da6506135c66981dd81d363618530adaab1c1c8bc6ba098fa2262ee1d220620";
    JSONObject transactionInfo = ssn.getTransactionByHash(txHash);

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
    String txHash = "0x0da6506135c66981dd81d363618530adaab1c1c8bc6ba098fa2262ee1d220620";
    JSONObject receiptInfo = ssn.getReceiptByHash(txHash);

返回:
::
    receiptInfo：执行结果
	
获取联盟链已注册合约列表
::::::::::::::::::::::::::::::::::


代码:
::
    JSONArray dappList = ssn.getContractAddrList();

返回:
::
    dappList: 合约列表
    
获取联盟链账户余额
::::::::::::::::::::::::::::::::::


参数:
::
    address: 账户地址

代码:
::
    String address = "0xf9efdec0293f5742c24ea35b1bfa0f341bdea51d";
    BigDecimal balance = ssn.getBalance(address);

返回:
::
    balance：联盟链账户余额（erc20最小单位）
    
    
获取Nonce
:::::::::::::::::::::::::


参数:
::
    addr：账户钱包地址

代码:
::
    String address = "0xf9efdec0293f5742c24ea35b1bfa0f341bdea51d";
    int nonce = ssn.getNonce(address);

返回:
::
    nonce: 得到的nonce
    
获取联盟链详细信息
:::::::::::::::::::::::::

代码:
::
    JSONObject appChainInfo = ssn.getAppChainInfo();

返回:
::
    appChainInfo: 联盟链信息

   
调用联盟链合约
::::::::::::::::::::::::

参数:
::
    contractAddress: dapp合约地址
    arry：例如合约中存在一个无参的方法getDechatInfo, JSONArray arry = new JSONArray(); arry.add("getDechatInfo");
             存在一个有参的方法JSONArray arry = new JSONArray(); arry.add("balanceOf"); arry.add("0x708678c49fe4e99fb8ccb487b1f667800ee2ecc8");

代码:
::
    String contractAddress = "0xf137f9efe9b6707ea218d1ea9a1697e54b75c82c";
    JSONArray arry = new JSONArray();
    arry.add("balanceOf");
    arry.add("0x708678c49fe4e99fb8ccb487b1f667800ee2ecc8");
    JSONObject anyCallInfo = ssn.anyCall(contractAddress, arry);

返回:
::
    anyCallInfo：调用合约返回信息
    
调用墨珩联盟链合约
::::::::::::::::::::::::

参数:
::
    contractAddress: dapp合约地址
    funcName 合约方法名 例 "balanceOf"
    list 合约方法及参数 列表 例 List list = new ArrayList(); list.add(new Address("0x44c10f4cd26dbb33b0cc3bd8d9fb4e313498cfa0"));

代码:
::
    String contractAddress = "0xf137f9efe9b6707ea218d1ea9a1697e54b75c82c";
    String funcName = "balanceOf";
    List list = new ArrayList();
    list.add(new Address("0x708678c49fe4e99fb8ccb487b1f667800ee2ecc8"));
    JSONObject callInfo = ssn.call(contractAddress, "balanceOf", list);

返回:
::
    callInfo：调用合约返回信息


联盟链加签交易
:::::::::::::::::::::::::

参数:
::
    from：发送方的钱包地址
    contractAddress：联盟链合约地址
    amount：交易金额
    method：方法 例 "issue"
    list: 合约方法及参数 列表 例 List list = new ArrayList(); list.add(new Address("0x44c10f4cd26dbb33b0cc3bd8d9fb4e313498cfa0"));
    privateKey：发送方钱包私钥
    nonce：发送方账户nonce(非必填)

代码:
::
    String from = "0xabd9e6c9951c9377171d2977d0b11383723926f1";
    String contractAddress = "0xa8195da48ddf8919e7ccf4e4417a57da8d95469d";
    String amount = 1;
    String method = "issue";
    List list = new ArrayList();
    list.add(new Address("0x708678c49fe4e99fb8ccb487b1f667800ee2ecc8"));
    String privateKey = "0x8be2254b13b0f00036f68832b561a679f1b5171638b6e9d5e8656baa2d1ceddc";
		
    String hash = ssn.sendRawTransaction(from, contractAddress, amount, method, list, privateKey);


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
    String from = "0xabd9e6c9951c9377171d2977d0b11383723926f1";
    String to = "0xa8195da48ddf8919e7ccf4e4417a57da8d95469d";
    String amount = 1;
    Stting data = "test";
    String privateKey = "0x8be2254b13b0f00036f68832b561a679f1b5171638b6e9d5e8656baa2d1ceddc";
    
    String hash = ssn.sendRawTransactionTransfer(from, to, amount, data, privateKey);

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
    method：方法 例 "issue"
    list: 合约方法及参数 列表 例 List list = new ArrayList(); list.add(new Address("0x44c10f4cd26dbb33b0cc3bd8d9fb4e313498cfa0"));
    privateKey：发送方钱包私钥

代码:
::
    String signedTx = ssn.sendRawTransaction(from, contractAddress, amount, method, list, privateKey);

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
    String signTx = "0xf90a508080808094a8195da48ddf8919e7ccf4e4417a57da8d95469d880de0b6b3a7640000b909e4b5560a14000000000000000000000000d92bbb39d372e2b752c6a0c9ea3351cd3c2a3e77000000000000000000000000abd9e6c9951c9377171d2977d0b11383723926f1000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000009525b7b22636f6e7374616e74223a747275652c22696e70757473223a5b5d2c226e616d65223a226e616d65222c226f757470757473223a5b7b226e616d65223a22222c2274797065223a22737472696e67227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a66616c73652c22696e70757473223a5b7b226e616d65223a225f7370656e646572222c2274797065223a2261646472657373227d2c7b226e616d65223a225f76616c7565222c2274797065223a2275696e74323536227d5d2c226e616d65223a22617070726f7665222c226f757470757473223a5b7b226e616d65223a2273756363657373222c2274797065223a22626f6f6c227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a226e6f6e70617961626c65222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b5d2c226e616d65223a22746f74616c537570706c79222c226f757470757473223a5b7b226e616d65223a22222c2274797065223a2275696e74323536227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a66616c73652c22696e70757473223a5b7b226e616d65223a225f66726f6d222c2274797065223a2261646472657373227d2c7b226e616d65223a225f746f222c2274797065223a2261646472657373227d2c7b226e616d65223a225f76616c7565222c2274797065223a2275696e74323536227d5d2c226e616d65223a227472616e7366657246726f6d222c226f757470757473223a5b7b226e616d65223a2273756363657373222c2274797065223a22626f6f6c227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a226e6f6e70617961626c65222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b5d2c226e616d65223a22494e495449414c5f535550504c59222c226f757470757473223a5b7b226e616d65223a22222c2274797065223a2275696e74323536227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b5d2c226e616d65223a22646563696d616c73222c226f757470757473223a5b7b226e616d65223a22222c2274797065223a2275696e7438227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b7b226e616d65223a225f6f776e6572222c2274797065223a2261646472657373227d5d2c226e616d65223a2262616c616e63654f66222c226f757470757473223a5b7b226e616d65223a2262616c616e6365222c2274797065223a2275696e74323536227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b5d2c226e616d65223a2273796d626f6c222c226f757470757473223a5b7b226e616d65223a22222c2274797065223a22737472696e67227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a66616c73652c22696e70757473223a5b7b226e616d65223a225f746f222c2274797065223a2261646472657373227d2c7b226e616d65223a225f76616c7565222c2274797065223a2275696e74323536227d5d2c226e616d65223a227472616e73666572222c226f757470757473223a5b7b226e616d65223a2273756363657373222c2274797065223a22626f6f6c227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a226e6f6e70617961626c65222c2274797065223a2266756e6374696f6e227d2c7b22636f6e7374616e74223a747275652c22696e70757473223a5b7b226e616d65223a225f6f776e6572222c2274797065223a2261646472657373227d2c7b226e616d65223a225f7370656e646572222c2274797065223a2261646472657373227d5d2c226e616d65223a22616c6c6f77616e6365222c226f757470757473223a5b7b226e616d65223a2272656d61696e696e67222c2274797065223a2275696e74323536227d5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a2276696577222c2274797065223a2266756e6374696f6e227d2c7b22696e70757473223a5b5d2c2270617961626c65223a66616c73652c2273746174654d75746162696c697479223a226e6f6e70617961626c65222c2274797065223a22636f6e7374727563746f72227d2c7b22616e6f6e796d6f7573223a66616c73652c22696e70757473223a5b7b22696e6465786564223a747275652c226e616d65223a225f66726f6d222c2274797065223a2261646472657373227d2c7b22696e6465786564223a747275652c226e616d65223a225f746f222c2274797065223a2261646472657373227d2c7b22696e6465786564223a66616c73652c226e616d65223a225f76616c7565222c2274797065223a2275696e74323536227d5d2c226e616d65223a225472616e73666572222c2274797065223a226576656e74227d2c7b22616e6f6e796d6f7573223a66616c73652c22696e70757473223a5b7b22696e6465786564223a747275652c226e616d65223a225f6f776e6572222c2274797065223a2261646472657373227d2c7b22696e6465786564223a747275652c226e616d65223a225f7370656e646572222c2274797065223a2261646472657373227d2c7b22696e6465786564223a66616c73652c226e616d65223a225f76616c7565222c2274797065223a2275696e74323536227d5d2c226e616d65223a22417070726f76616c222c2274797065223a226576656e74227d5d00000000000000000000000000000180820bf0a05ff32d9aa49a4be189c94cef15f835dedb8db9838ff0ed11b879b7611825e250a00d3d2d1e35ff3917b776a6bf68b2a1d7966aa4de5c09bba53873efd348c06044";
    
    String hash = ssn.sendSignTransaction(signTx);

返回:
::
    hash：交易hash





