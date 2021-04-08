混合支付平台 -- SL21系统
--------------------------

.. image:: ../imgs/todo.png
  :align: center
  :width: 200px

服务介绍
~~~~~~~~
..
    本系统各模块采用分布式部署方式，相互之间通过基于tcp的rpc服务调用，具体分为如下四个服务：

    1. **bcs** :
    存证溯源业务服务，包含扫码（锁定标签，标签存证，标签解锁），溯源信息查询，溯源校验等功能模块，可对外提供api服务，默认端口3001。

    2. **traceToChain** :
    溯源信息上链服务，bcs服务会定时调用此服务将扫码溯源信息批量上链，默认端口3002。

    3. **merkleMemory** : 
    公共内存服务，用于构建ctree结构, 默认端口3000。 

    4. **checkChainResult** :
    上链结果确认服务，此服务是定时任务，用来确保上链业务db中数据和链上溯源数据的一致性，默认端口3004。

准备环境
~~~~~~~~
..
    -  保证您的主机已经安装node环境,
       版本不低于v10.0，如还未安装，请参考\ `这里`_
    -  保证您的主机已经安装node express环境, 版本不低于v6.0,
       如还未安装，请参考\ `这里 <http://expressjs.com/en/starter/installing.html>`__
    -  保证**bcs**服务器已经安装mongodb,
       版本不低于v3.6，如还未安装，请参考\ `这里 <https://docs.mongodb.com/manual/installation/>`__
    -  保证您有可连接的  :doc:`墨珩联盟链 <../supersolid/Supersolid/index>` 环境

依赖安装
~~~~~~~~

环境配置完成后，到各个服务项目的根目录下，执行如下指令安装依赖包。

::

   npm install

配置文件
~~~~~~~~
..
    依赖安装完成后，请根据自己需求，修改各服务根目录配置文件(config.json),
    如下为配置文件各属性含义说明：

    -  mongoHost - mongodb数据库服务器，格式为ip:port(如121.43.129.11:10010)
    -  mongouname - mongodb数据库服务器用户名
    -  mongopasswd - mongodb数据库服务器密码
    -  dbname - 当前项目数据库名称
    -  ssbHost - SSB服务器连接方式，格式为ip:port(如120.78.146.128:8545)
    -  baseAddress - vss_base地址
    -  ssnHost - SSN服务器连接方式
    -  rpcHost - merkleMemory内部rpc服务连接方式, 默认为“127.0.0.1:4242”，请勿修改
    -  from - 交易发送方, 及联盟链合约owner
    -  to - 交易接受方
    -  privateKey - 交易发送方私钥
    -  env - 运行环境类型，“demo”为前台演示环境，“product”为扫码生产环境
    -  limitNum - 建议单次上链信息数量，默认为12000
    -  pkStrStart - 公钥格式符开头，默认为“—–BEGIN PUBLIC KEY—–”，请勿修改
    -  pkStrEnd - 公钥格式符结束，默认为“—–END PUBLIC KEY—–”，请勿修改
    -  openInfo - 出场状态信息，默认为“open”，请勿修改

    以上凡涉及host格式，请勿在开头添加http://, 保证“:”前后无空格。

日志文件
~~~~~~~~

..
    本系统日志文件采用nodejs
    log4j配置，具体配置项可到各个服务根目录下log4js.json文件中查看，默认日志文件路径为根目录下/log文件夹，日志文件名为server-日期.log。

启动
~~~~
..
    以上安装与配置都完成后，请开启各个服务端口，它们默认为：3000，3001，3002，3004，4242，您也可以到各服务项目根目录下app-*.js文件中修改端口（实际情况下请将
    \* 替换为各个服务名称）。分别到各个服务根目录启动项目，如下：

    ::

       node app-*

    启动后在浏览器输入：http://localhost:3001, 方可查看项目启动情况,
    除此之外，您还可以根据自己情况，配置pm2方式启动。

    .. _这里: https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

