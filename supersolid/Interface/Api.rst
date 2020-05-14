Restful API 简介
-----------------------------

为了简化DAPP的应用，我们以Restful API的方式提供给用户一种便捷的接入方法。通过HTTP对SDK的基础操作进行一系列封装，来实现对用户不同编程语言的兼容。

DAPP用户当调用客户端SDK有困难时，可以通过服务端调用的方式实现对联盟链SDK的接入。


URL设计
>>>>>>>>>>>>>>>>>>>>>>>>>

使用http作为API的通信协议，目前采用较多的POST方法。

url格式:    http(s)://server.com/api/{module}/{version}/{method}

关于version，各模块支持多版本，默认version为v1.0

POST提交格式采用form表单参数    Content-Type: application/x-www-form-urlencoded 


结构返回
>>>>>>>>>>>>>>>>>>>>>>>>>
返回体格式采用json格式
::	
	{
		"success": true,
		"message": "",
		"data": "*********"
	}
	success:  	true - 成功    false - 失败
	message: 	失败时的错误信息
	data:	  	接口返回数据

相关状态码

200 OK

403 token权限受限

404	接口不存在



接口访问控制
>>>>>>>>>>>>>>>>>>>>>>>>>

API访问控制，需先请求访问的用户名和密码，先调用auth接口请求访问token。

各api接口需要token参数调用，不然会返回403错误。

token有过期机制，目前是2小时有效。


私钥安全
>>>>>>>>>>>>>>>>>>>>>>>>>

对于账号私钥的安全提供了两种方案。

1. dapp注册账户后，收到返回的私钥（privatekey），后续发送交易直接传递私钥
2. dapp注册账户后，会返回一个加密串（encode），后续发送交易传递加密串和账户密码，系统会解码获得私钥进行签名。


节点控制
>>>>>>>>>>>>>>>>>>>>>>>>>

测试环境地址：http://139.198.126.104:8088

测试环境获取access token，可使用测试账号：test    密码：123456

正式环境地址：https://api.moac.io:8088

正式环境获取access token，请联系开发团队：moacapi@mossglobal.net。

目前设计需要ssn节点的相关API，可通过参数传入地址和端口的方式指定连接的节点。
参数传入为空的情况下，会使用平台默认的节点信息（测试环境对应测试链的默认节点 59.111.104.28:8647，正式环境默认主网节点）。


Restful API 接口
>>>>>>>>>>>>>>>>>>>>>>>>>

API认证
:::::::::::::::::::::::::::::::::

请求访问token，提供权限调用API的其他接口

方法：auth

参数:
::
	account:  	授权账号
	pwd:  		授权账号密码
	
调用示例：
::
	POST: http://139.198.126.104:8088/auth
	BODY：account=******&pwd=******

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": "token内容"
	}
	
账户注册
:::::::::::::::::::::::::::::::::

方法：register

参数:
::
	pwd:  账户密码
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/account/v1.0/register
	BODY：pwd=********&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"address": 账户地址,
		"encode": 账户加密串,
		"keystore": 账户keystore信息,
		"privateKey": 账户私钥
	}
	
账户登录
:::::::::::::::::::::::::::::::::

方法：login

参数:
::
	address:  	账户地址
	pwd:  		账户密码
	encode:  	账户加密串
	token: 		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/account/v1.0/login
	BODY：address=0x********&pwd=*****&encode=*******&token=************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户地址
	}
	
账户余额
:::::::::::::::::::::::::::::::::

方法：getBalance

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	address:  	账号地址
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getBalance
	BODY：ssnip=127.0.0.1&ssnport=8545&address=0x******&token=*****************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额 (联盟链原生币数量)	
	}
	
区块高度
:::::::::::::::::::::::::::::::::

方法：getBlockNumber

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getBlockNumber
	BODY：ssnip=127.0.0.1&ssnport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块高度
	}	
	
下次续费区块高度
:::::::::::::::::::::::::::::::::

方法：getBlockThreshold

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getBlockThreshold
	BODY：ssnip=127.0.0.1&ssnport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 续费区块高度
	}	
	
联盟链ssnID
:::::::::::::::::::::::::::::::::

方法：getSsnID

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getSsnID
	BODY：ssnip=127.0.0.1&ssnport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 联盟链ssnID
	}		
	
获得联盟链已注册合约列表
:::::::::::::::::::::::::::::::::

方法：getContractAddrList

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getContractAddrList
	BODY：ssnip=127.0.0.1&ssnport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 合约列表
	}	
	
获得联盟链详细信息
:::::::::::::::::::::::::::::::::

方法：getAppChainInfo

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getAppChainInfo
	BODY：ssnip=127.0.0.1&ssnport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 联盟链详细信息
	}		
	
区块信息
:::::::::::::::::::::::::::::::::

方法：getBlock

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	blocknum:  	区块号或者区块hash
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getBlock
	BODY：ssnip=127.0.0.1&ssnport=8545&blocknum=10036&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块信息
	}	

交易明细
:::::::::::::::::::::::::::::::::

方法：getTransactionByHash

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	hash:  		交易hash
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getTransactionByHash
	BODY：ssnip=127.0.0.1&ssnport=8545&hash=0x**&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易明细
	}

交易详情
:::::::::::::::::::::::::::::::::

方法：getTransactionReceiptByHash

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	hash:  		交易hash
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/getTransactionReceiptByHash
	BODY：ssnip=127.0.0.1&ssnport=8545&hash=0x**&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易详情
	}	
	
转账
:::::::::::::::::::::::::::::::::

方法：transferCoin

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	from:  		源账号地址
	to:  		目标账号地址
	amount:  	数量（联盟链原生币数量）
	data:  		备注信息
	privatekey: 源账号私钥 （传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 		账户密码
	encode：	账户加密串
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/transferCoin
	BODY：ssnip=127.0.0.1&ssnport=8545&from=0x**&to=0x***&amount=10&data=*****&privatekey=0x**&token=*******

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

调用智能合约
:::::::::::::::::::::::::::::::::

方法：callContract

参数:
::
	ssnip:  	联盟链节点地址
	ssnport:  	联盟链节点端口
	contractaddress:  合约地址
	param:  	例如合约中存在一个无参的方法getDechatInfo，则传入["getDechatInfo"];
				若存在一个有参的方法getTopicList(uint pageNum, uint pageSize), 则传入["getTopicList", 0, 20]
	token:  	auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8088/api/ssn/v1.0/callContract
	BODY：ssnip=127.0.0.1&ssnport=8545&contractaddress=0x*****&param=["getTopicList", 0, 20]&token=***********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 调用合约返回结果
	}		
	
	
	
	
	
	
	
	
	
	
	
	
	