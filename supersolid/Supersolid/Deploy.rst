联盟链部署
--------------------------

在本节中，我们将采用一个启动节点 + 五个主节点的配置来描述如何部署墨珩联盟链。

启动SSB
>>>>>>>>>>>>>>>>>>>>>>>>>>

**1.1** 将安装包中的 super-solid-base 和 genesis.json 放在同一目录下，在服务器终端输入
::
    super-solid-base  init genesis.json  --datadir "./path/to/chaindata"

**datadir**：表示初始化联盟链数据的目录。

.. |br| raw:: html

    <br/>

|br|

**1.2** 启动SSB
::
    super-solid-base --verbosity=4, --rpc, --networkid=1510, --datadir=./path/to/chaindata, 
    --rpcaddr=0.0.0.0, --rpcport=8545, --rpcapi=chain3,mc,net,db


可修改的参数说明如下

**verbosity**：日志级别（debug:4, info:3）。

**datadir**：表示初始化联盟链数据的目录。

|br|

**1.3** 在服务器新窗口中，输入
::
    super-solid-base attach

此时进入SSB节点的命令行模式，这个命令行模式（以下简称SSB命令行）将在部署过程中一直使用，请不要关闭，如果异常，请再次attach进入。

|br|

**1.4** 创建 :doc:`owner <../Definitions>` 账号。在SSB命令行中输入
::
    >personal.newAccount("YourPwd")

其中，YourPwd是指owner账号的密码，返回是owner的地址。

|br|

**1.5** 命令行输入
::
    >miner.start(1)

此时SSB将会进入挖矿状态。

|br|

**1.6** 等待一段时间，在SSB命令行输入
::
    >mc.blockNumber

当输出值大于10以后，表示SSB启动成功。

------------------------------------------------------------------------------------------

部署SSB合约vss_base
>>>>>>>>>>>>>>>>>>>>>>>>>>

**2.1** 将安装包中vssbase.js放到SSB服务器上，打开js文件，配置联盟链初始化参数
::
    var min = 0;                // 暂时无效
    var max = 5 ;               // 联盟链初始主节点数量
    var tokensupply = 1000000 ; // 联盟链原生币数量
    var owner = "0x...";        // owner地址
    var pwd = "YourPwd";        // owner密码

**max**：标识联盟链初始主节点数量，建议是5，7，9，11。只有达到这个数量，才能建立联盟链。

**tokensupply**：联盟链原生币数量，建立联盟链后，原生币将会打入owner账号。

**owner**：1.4中主账号地址。

|br|

**2.2** SSB命令行输入
::
    >loadScript("your/path/to/vssbase.js")

等待结果返回 
::
    Contract mined! address: 0x...

此时标识部署vss_base合约成功。记录此地址后续备用。

-----------------------------------------------------------------------------------------

首次次启动SSN节点
>>>>>>>>>>>>>>>>>>>>>>>>>>

**3.1** 将安装包中userconfig.json放到SSN服务器上，配置userconfig.json
::
    {
    "RpcServiceCfg": "http://127.0.0.1:8545/rpc",      
    "DataDir": "./ssndata",                                                   
    "LogPath": "./_logs",                                                      
    "VssBaseAddr": "0x...",    
    "ChainId": 1510,                                //无需修改
    "LogLevel": 4,
    "SuperSolidName": "myssname"                                                       
    }

**RpcServiceCfg**：SSB rpc接口地址，要与1.2启动SSB相一致

**DataDir**：SSN 数据路径

**LogPath**：SSN 日志路径

**VssBaseAddr**：vss_base合约地址

**LogLevel**：节点日志级别（debug:4, info:3）

**SuperSolidName**：联盟链名称，长度不得超过32个字节，**请注意**，所有SSN节点的SuperSolidName必须一致！


|br|

**3.2** 将安装包中的 super-solid-node 和 userconfig.json 放在同一目录下，在服务器终端输入
::
    super-solid-node --rpc --rpcaddr 0.0.0.0 --rpcport 8546 --p2pport 30383

**rpcaddr**：SSN rpc地址

**rpcport**：SSN rpc端口

**p2pprot**：SSN p2p端口

第一次启动后，super-solid-node会自动关闭并提示ssnId not sufficient funds。

在super-solid-node可执行文件路径下找到 **ssnkeystore** 文件夹，获取ssnId（第一个keystore文件的address），记录这个ssnid备用。

在super-solid-node可执行文件路径下找到 **ssndata/nodes** 文件夹，文件夹里有my-static-node.json文件。

my-static-node.json文件示例如下
::
    ["enode://00137f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999081e08ad@[ip]:30383"]

**请注意：** 如果联盟链建立在内网中，可以将ip改成内网ip；如果是外网环境，须将ip改成外网ip。

记录这个enode信息备用。

|br|

**3.3** 重复3.1和3.2，将其他SSN节点启动起来，并记录各自的ssnid和enode。

**请注意：** 启动的SSN数量必须和2.1 vssbase 中的max数量相等。

|br|

**3.4** 将汇总的enode信息做成一个总的static-nodes.json放到所有SSN节点的 **ssndata/nodes/** 下。

总的static-nodes.json文件示例如下
::
    ["enode://00137f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999081e08ad@[ip]:30383",
    "enode://00237f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999082e08ad@[ip]:30383",
    "enode://00337f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999083e08ad@[ip]:30383",
    "enode://00437f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999084e08ad@[ip]:30383",
    "enode://00537f199db5239989d3f2e2c1a2......a96c81a81321c5465682fc240e49a5a4d9999085e08ad@[ip]:30383"]

------------------------------------------------------------------------------------------------------------

SSN节点添加gas
>>>>>>>>>>>>>>>>>>>>>>>>>>

**4.1** 将安装包中sendgas.js放到SSB服务器上，打开js文件
::
    var ssnaddrs=["0x...", "0x...", "0x...", "0x...", "0x..."]; 

|br|

**4.2** SSB命令行输入
::
    >loadScript("your/path/to/sendgas.js")

等待结果返回 
::
    Success address: 0x..., Balance: 100
    Success address: 0x..., Balance: 100
    Success address: 0x..., Balance: 100
    Success address: 0x..., Balance: 100
    Success address: 0x..., Balance: 100

如上信息表示添加gas成功！

-----------------------------------------------------------------------------------------------------------

再次启动SSN节点
>>>>>>>>>>>>>>>>>>>>>>>>>>

**5.1** 再次一次启动所有SSN节点
::
    super-solid-node --rpc --rpcaddr 0.0.0.0 --rpcport 8546 --p2pport 30383

此时SSN不会退出，将会进入正常的启动流程。

|br|

**5.2** 选择一个SSN节点，新开一个服务器窗口，输入
::
    super-solid-node attach

进入SSN命令行模式，等待一段时间，输入
::
    >mh.blockNumber

当输出值大于1以后，表示联盟链启动成功！！

**！！至此联盟链全部部署完成！！**

此时，owner地址中将会有totalsupply的货币数额，可在SSN的命令行输入如下命令查询
::
    > mh.getBalance(youroweneraddr)

..
    ---------------------------------------------------------------------------------------------------------

    部署SSN合约dapp_base
    >>>>>>>>>>>>>>>>>>>>>>>>>>

    **6.1** 将安装包中dappbse.js放到SSN服务器上，打开js文件
    ::
        var dappname = "mydappname";      

    **dappname**：联盟链名称

    |br|

    **6.2** SSN命令行输入
    ::
        >loadScript("your/path/to/dappbase.js")

    执行完后，会返回：tx hash: 0x......。

    等待一段时间，然后SSN命令行输入
    ::
        >mh.getReceiptByHash('0x......')

    如果返回null则继续等待，有结果返回，并且有contractAddress，failed = false，则表示dappbase部署成功。

    |br|

    **！！至此联盟链全部部署完成！！**

    |br|

|br|
|br|

接下来可继续部署  :doc:`联盟链浏览器 <../Explorer>` 和 :doc:`联盟链监控 <../Monitor>`。

|br|
|br|

------------------------------------------------------------------------------------------------

部署注意点
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
在云服务上开启相关的rpc端口