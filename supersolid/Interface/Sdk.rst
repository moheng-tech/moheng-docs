联盟链SDK介绍
-----------------------------

SDK是作为专为客户端使用的软件开发包，极大方便了开发人员在客户端直接调用区块链相关接口。当前SDK可提供nodejs版本。SDK可同时连接 :doc:`SSN <../Definitions>` 和SSB。

另外两种 :doc:`Console <./Console>` 和 :doc:`RPC <./Rpc>` 的开发模式，将在本章其他节介绍。

SDK的安装
>>>>>>>>>>>>>>>>>>>>>>>>>>

可使用如下命令安装联盟链nodejs版本的SDK
::
    npm install super-solid-link

nodejs版SDK详述
>>>>>>>>>>>>>>>>>>>>>>>>>>

异常处理
::::::::::::::::::::

通常，采用如下方式进行异常的捕获
::
    var ssb = require("super-solid-link").ssb;

    try{
            var ssb = new ssb("http://127.0.0.1:8545");
            var blockNumber = ssb.getBlockNumber();
            console.log(blockNumber);
    }catch (e){
            console.log(e);
    }




