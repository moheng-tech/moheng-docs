联盟链监控
-----------------------------

.. image:: ../imgs/Super-monitor.png
  :align: center

什么是监控
>>>>>>>>>>>>>>>>>>>>>>>>>>

墨珩联盟链监控(Super-Monitor)，以下简称监控，是墨珩联盟链平台的一个配套服务。在一个已经部署的联盟链上，可以快速部署这样一套监控系统来监控联盟链的数据。

监控系统有如下一些模块

- 账户模块，监控根据不同的登陆账号给出不同的权限
- 数据显示模块
- 图表模块
- 操作模块，主要收费续块和节点相关操作
- 报警模块
- 日志模块等

服务器配置推荐
>>>>>>>>>>>>>>>>>>>>>>>>>>

推荐2核2G服务器一台，40G硬盘。

推荐监控服务器和联盟链在同一网络环境下，可走内网。如果走外网，需要至少2M带宽。


监控部署
>>>>>>>>>>>>>>>>>>>>>>>>>>

**强烈建议安装在Ubuntu中**

部署前需要安装的软件
::::::::::::::::::::::::::

在部署监控前，以下软件必须安装

- node：https://npm.taobao.org/mirrors/node/v10.19.0/，推荐下载v10.19.0
- npm：node自带
- mongodb：https://www.mongodb.org/dl/linux
- pm2

程序包下载
::::::::::::::::::::::::::

联盟链监控的程序包可在如下连接中获取: |location_link| 。

.. |location_link| raw:: html

   <a href="https://www.baidu.com" target="_blank">监控程序包</a>


依赖包安装
::::::::::::::::::::::::::

解压程序包到安装目录下，并执行
::
    npm install


修改配置文件
::::::::::::::::::::::::::

**Mongodb配置**

配置根目录下Mongodb文件config.json

- mongoHost：mongo IP地址
- mongouname：mongo用户名 
- mongopasswd：mongo密码
- dbname：mongo库名称

**Monitor配置**

- baseAddress：ssb地址

- ssb->list->url：ssb的地址和端口号

- ssn->list->url：ssb的地址和端口号

- Server->对应ssb和ssn的服务器
- Server->name：前台页面需要展示的名称
- Server->show(1：前台页面展示，0：前台页面不展示)
- Server->host：服务器的IP地址
- Server->port：服务器的端口
- Server->username：服务器的用户名
- Server->serverPrivateKey：服务器私钥文件（在项目routes目录下，若每台服务器私钥不一样需要对应不同的名称）
- Server->montiorCmd：监控服务器信息的脚本绝对路径
- Server->restartCmd：重启ssb服务或者ssn服务的脚本绝对路径
- Server->myNodePath：ssn服务的node绝对路径
- Server->allNodePath：ssn服务p2p连接文件的绝对路径

- Html->name：页面左菜单显示的名称
- Html->chart_url:页面图表嵌入的网址

启动并查看监控
::::::::::::::::::::::::::

输入如下命令启动监控
::
    pm2 start app.js

至此，可在 http:本机ip:3002 查看监控。



监控的信息介绍
>>>>>>>>>>>>>>>>>>>>>>>>>>

监控的使用介绍
>>>>>>>>>>>>>>>>>>>>>>>>>>

续块
::::::::::::::::::

节点添加
::::::::::::::::::

**联盟链拥有者**

**老节点方**

**新节点方**

转账
::::::::::::::::::

重启节点
::::::::::::::::::

