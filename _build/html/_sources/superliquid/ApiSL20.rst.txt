混合支付平台 -- SL20 API 接口说明
---------------------------------------



查询模块
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

查询某账户的所有子账户列表
::::::::::::::::::::::::::::::::::


**简要描述：**

-  查询某账户下，所有子账户列表，包括余额为零的子账户。数字资产在SL20平台有子账户模式，子账户的调度由系统自动完成，用户只需关注总额即可。

**请求URL：** - ``http://localhost:3001/getUserTokens``

**请求方式：** - POST

**参数：**

========== ==== ====== ============
参数名     必选 类型   说明
========== ==== ====== ============
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
========== ==== ====== ============

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "balanceArr": [{
               "balance": 0,
               "balanceType": "record",
               "owner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "value": "0,0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "updateTime": 1591955961935,
               "proof": "[{\"left\":\"bc52dd634277c4a34a2d6210994a9a5e2ab6d33bb4a3a8963410e00ca6c15a02\"}]",
               "status": 1,
               "key": 165
           }],
           "totalBalance": 0
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

============ ====== ===============
data参数名   类型   说明            
============ ====== ===============
key          number 子账户编号      
balance      number 子账户余额      
proof        string 子账户proof路径 
owner        string 子账户拥有者    
value        string 当前value       
updateTime   number 最近更新时间    
status       number 子账户拥有者    
totalBalance number 总账户余额      
============ ====== ===============

**状态码**

  服务异常状态码 

  - 501：db查询异常 
  - 502：db插入或更新异常
  - 503：上链交易失败 
  - 504：设置缓存失败 
  - 505: 获取缓存失败 
  - 506：登录失败
  - 507：rpc服务调用失败

  业务异常状态码 

  - 600：解析用户信息失败 
  - 601：已经有初始化token
  - 602：key不合法 
  - 603：余额不足 
  - 604：提取合法性校验失败
  - 605：当前token拥有者异常 
  - 606：验签失败 
  - 607：参数异常
  - 608：接收人地址不存在 
  - 609：接收人无初始化子账户
  - 610：当前账户无上链余额

------------------------------------------------------------------------------------

查询用户维度转账记录
::::::::::::::::::::::::::::::::::

**简要描述：**

-  查询用户SL20快速支付交易记录

**请求URL：** - ``http://localhost:3001/getUserTransferList``

**请求方式：** - POST

**参数：**

========== ==== ====== ============
参数名     必选 类型   说明
========== ==== ====== ============
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
page       是   number 当前第几页
rows       是   number 每页多少条
========== ==== ====== ============

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "totalNum": 2,
           "dataList": [{
               "_id": "5ee35b72bb788219aed8bfde",
               "uniqueNo": "1591958386988",
               "fromOwner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "updateTime": 1591958386988,
               "amount": 10,
               "type": 1,
               "status": 0,
               "__v": 0
           }, {
               "_id": "5ee35b67bb788219aed8bfdb",
               "uniqueNo": "1591958375679",
               "fromOwner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "updateTime": 1591958375679,
               "amount": 10,
               "type": 1,
               "status": 0,
               "__v": 0
           }]
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       boolean 查询信息     
========== ======= ============

========== ====== ========================================
data参数名 类型   说明                                     
========== ====== ========================================
uniqueNo   number 唯一编码                                 
fromOwner  string 支付方地址                               
toOwner    string 接收方地址                               
updateTime number 转账时间                                 
amount     number 转账金额                                 
type       number 类型（1-支付 2-收款）                    
status     number 状态（0-等待上链 1-上链成功 2-等待确认） 
========== ====== ========================================

**状态码**

  服务异常状态码 

  - 501：db查询异常 
  - 502：db插入或更新异常
  - 503：上链交易失败 
  - 504：设置缓存失败 
  - 505: 获取缓存失败 
  - 506：登录失败
  - 507：rpc服务调用失败

  业务异常状态码 

  - 600：解析用户信息失败 
  - 601：已经有初始化token
  - 602：key不合法 
  - 603：余额不足 
  - 604：提取合法性校验失败
  - 605：当前token拥有者异常 
  - 606：验签失败 
  - 607：参数异常
  - 608：接收人地址不存在 
  - 609：接收人无初始化子账户
  - 610：当前账户无上链余额

------------------------------------------------------------------------------------

获取当前可用子账户列表
::::::::::::::::::::::::::::::::::

**简要描述：**

-  获取当前可用子账户列表，不含余额为零的账户（转账和提取操作前调用）

**请求URL：** - ``http://localhost:3001/getBalanceWhenTransfer``

**请求方式：** - POST

**参数：**

========== ==== ====== ===============================
参数名     必选 类型   说明
========== ==== ====== ===============================
userAddr   是   string 发送人/当前用户
toOwner    是   string 接收人（提取时传空字符串""）
loginToken 是   string 当前登录凭证
type       是   number 操作类型（1-转账/支付，2-提取）
========== ==== ====== ===============================

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "balanceArr": [{
               "key": 163,
               "balance": 100,
               "proof": "[{\"left\":\"3d3286f7cd19074f04e514b0c6c237e757513fb32820698b790e1dec801d947a\"}]"
           }, {
               "key": 161,
               "balance": 0,
               "proof": "[{\"left\":\"bb668ca95563216088b98a62557fa1e26802563f3919ac78ae30533bb9ed422c\"}]"
           }, {
               "key": 162,
               "balance": 100,
               "proof": "[{\"left\":\"79d6eaa2676189eb927f2e16a70091474078e2117c3fc607d35cdc6b591ef355\"}]"
           }],
           "toKey": 139
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

========== ====== =================================
data参数名 类型   说明                              
========== ====== =================================
key        number 子账户编号                        
balance    number 子账户余额                        
proof      string 子账户proof路径                   
toKey      number 接收方子账户（提取操作时此值为0） 
========== ====== =================================

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常 
- 503：上链交易失败
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败 
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

---------------------------------------------------------------------------

查询子账户维度转账记录
::::::::::::::::::::::::::::::::::

**简要描述：**

-  查询子账户维度转账记录。 查询某个账户下子账户的转账记录。

**请求URL：** - ``http://localhost:3001/getTokenTransferList``

**请求方式：** - POST

**参数：**

========== ==== ====== ============
参数名     必选 类型   说明
========== ==== ====== ============
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
key        是   number 子账户
page       是   number 当前第几页
rows       是   number 每页多少条
========== ==== ====== ============

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "totalNum": 2,
           "dataList": [{
               "_id": "5ee35b72bb788219aed8bfdf",
               "key": 166,
               "uniqueNo": "1591958386990",
               "userTransferNo": "1591958386988",
               "fromKey": 166,
               "toKey": 139,
               "fromOwner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "keyHash": "e0f05da93a0f5a86a3be5fc0e301606513c9f7e59dac2357348aa0f2f47db984",
               "valueHash": "f9dc365824f56f533fd019bd3ef7ebae1d20fffda31756a4bd49b206e5fa99e5",
               "valueProof": "[{\"left\":\"e0f05da93a0f5a86a3be5fc0e301606513c9f7e59dac2357348aa0f2f47db984\"}]",
               "updateTime": 1591958386990,
               "txHash": "0xddb6218cee4f620be9c38c2c2cb2197177d9e4d4ca7168688abd620efce9adff",
               "rootHash": "0x5321537f0a2c43ce5961be733717f2d5b4f6269f03dca60f64e7902072f92be1",
               "amount": 10,
               "type": 1,
               "status": 1,
               "owner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "value": "100,0xe7557544513b68aabe6288b6c9da1d226ef8d8c0&debit,1591958375681,166,139,10,0xf486c721384c59d343620d2f65ee77b04bf1453ed7be9fc8981c9d8dbbb56938229d54bee4d5533f3110e0121d24ab7a839c0961db1d9cf36a5a403cad9d7d5e1c&debit,1591958386990,166,139,10,0xf486c721384c59d343620d2f65ee77b04bf1453ed7be9fc8981c9d8dbbb56938229d54bee4d5533f3110e0121d24ab7a839c0961db1d9cf36a5a403cad9d7d5e1c",
               "balance": 80,
               "__v": 0,
               "max": 166,
               "min": 166,
               "blockNumber": 78119
           }, {
               "_id": "5ee35b67bb788219aed8bfdc",
               "key": 166,
               "uniqueNo": "1591958375681",
               "userTransferNo": "1591958375679",
               "fromKey": 166,
               "toKey": 139,
               "fromOwner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "keyHash": "e0f05da93a0f5a86a3be5fc0e301606513c9f7e59dac2357348aa0f2f47db984",
               "valueHash": "9c9884ce2b10bbf257ba5671dc3f6abd0f8ee5be25f5643fb0baf2ccb03c1808",
               "valueProof": "[{\"left\":\"e0f05da93a0f5a86a3be5fc0e301606513c9f7e59dac2357348aa0f2f47db984\"}]",
               "updateTime": 1591958375681,
               "txHash": "0x0492b1666969a4324fcc48b8ad4016c66f254fa1c1b87db25684eb30fb49c975",
               "rootHash": "0xe4591a9482218602fdba783c114cf22d3a6cae2323f0a259aa19bf4dedd5c086",
               "amount": 10,
               "type": 1,
               "status": 1,
               "owner": "0xe7557544513b68aabe6288b6c9da1d226ef8d8c0",
               "value": "100,0xe7557544513b68aabe6288b6c9da1d226ef8d8c0&debit,1591958375681,166,139,10,0xf486c721384c59d343620d2f65ee77b04bf1453ed7be9fc8981c9d8dbbb56938229d54bee4d5533f3110e0121d24ab7a839c0961db1d9cf36a5a403cad9d7d5e1c",
               "balance": 90,
               "__v": 0,
               "max": 166,
               "min": 166,
               "blockNumber": 78118
           }]
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       boolean 查询信息     
========== ======= ============

============== ====== ========================================
data参数名     类型   说明                                     
============== ====== ========================================
key            number 子账户编号                               
uniqueNo       string 唯一编码                                 
userTransferNo string 用户转账唯一编码                         
fromKey        number 支付key                                  
toKey          number 接收key                                  
fromOwner      string 支付方地址                               
toOwner        string 接收方地址                               
keyHash        string 编号hash                                 
valueHash      string value的hash                              
valueProof     string 提现的proof                              
updateTime     number 转账时间                                 
txHash         string 上链交易hash                             
rootHash       string 上链交易hash                             
amount         number 转账金额                                 
type           number 类型（1-支付 2-收款）                    
status         number 状态（0-等待上链 1-上链成功 2-等待确认） 
owner          string token拥有者                              
value          string value明文                                
balance        number token余额                                
max            number 当前树子钱包最大编号                     
min            number 当前树子钱包最小编号                     
blockNumber    number 区块号                                   
============== ====== ========================================

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

--------------------------------------------------------------------------------

查询充值/提取记录
::::::::::::::::::::::::::::::::::

**简要描述：**

-  查询充值/提取记录

**请求URL：** - ``http://localhost:3001/getChargeOrWithdrawList``

**请求方式：** - POST

**参数：**

========== ==== ====== =====================
参数名     必选 类型   说明
========== ==== ====== =====================
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
type       是   number 类型（1-充值 2-提取）
page       是   number 当前第几页
rows       是   number 每页多少条
========== ==== ====== =====================

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "totalNum": 2,
           "dataList": [{
               "_id": "5ee3711351b73a19898a1eae",
               "owner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "key": 169,
               "uniqueNo": "1591963923170",
               "value": "100,0x731d00ede888d9435250545366a29396c5a5c96a",
               "keyHash": "f57e5cb1f4532c008183057ecc94283801fcb5afe2d1c190e3dfd38c4da08042",
               "valueHash": "d19705a27e0d230d78256959d3553d7f155ca5d9f9f3181d2ba29c0a247f160b",
               "valueProof": "[{\"left\":\"f57e5cb1f4532c008183057ecc94283801fcb5afe2d1c190e3dfd38c4da08042\"}]",
               "updateTime": 1591963923174,
               "txHash": "0xeb1136cdb3abc324442e404e3b47fe9d632b50a155297a0b67aba1f8c7b8c79d",
               "blockNumber": 78672,
               "rootHash": "0xd12c15cc091c4928fae3440ecf6f39896f945f06dfbec2f5465f6075d60488ef",
               "min": 169,
               "max": 169,
               "amount": 100,
               "balance": 100,
               "type": 1,
               "status": 1,
               "__v": 0
           }, {
               "_id": "5ee3710951b73a19898a1eac",
               "owner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "key": 168,
               "uniqueNo": "1591963913169",
               "value": "100,0x731d00ede888d9435250545366a29396c5a5c96a",
               "keyHash": "80c3cd40fa35f9088b8741bd8be6153de05f661cfeeb4625ffbf5f4a6c3c02c4",
               "valueHash": "d19705a27e0d230d78256959d3553d7f155ca5d9f9f3181d2ba29c0a247f160b",
               "valueProof": "[{\"left\":\"80c3cd40fa35f9088b8741bd8be6153de05f661cfeeb4625ffbf5f4a6c3c02c4\"}]",
               "updateTime": 1591963913172,
               "txHash": "0x2aeb338ee1bca43a149ec3c4e6a4b8286ecc657f4a0a50b8b7ace2dc00f5c00e",
               "blockNumber": 78671,
               "rootHash": "0x18dcdc6ac87dbff259e0570cca03290890fab6f9374c1741a0b5c1d727a3f543",
               "min": 168,
               "max": 168,
               "amount": 100,
               "balance": 100,
               "type": 1,
               "status": 1,
               "__v": 0
           }]
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       boolean 查询信息     
========== ======= ============

=========== ====== ========================================
data参数名  类型   说明                                     
=========== ====== ========================================
owner       string 子账户拥有者                             
key         number 子账户编号                               
uniqueNo    string 唯一编码                                 
value       string value明文                                
keyHash     string 编号hash                                 
valueHash   string value的hash                              
valueProof  string 提现的proof                              
updateTime  number 转账时间                                 
txHash      string 上链交易hash                             
blockNumber number 区块号                                   
rootHash    string 上链交易hash                             
min         number 当前树子钱包最小编号                     
max         number 当前树子钱包最大编号                     
amount      number 充值或提取金额                           
balance     number token余额                                
type        number 类型（1-支付 2-收款）                    
status      number 状态（0-等待上链 1-上链成功 2-等待确认） 
=========== ====== ========================================

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

--------------------------------------------------------------------

获取交易详细信息
::::::::::::::::::::::::::::::::::

**简要描述：**

-  获取交易详细信息

**请求URL：** - ``http://localhost:3001/getUserTransferDetail``

**请求方式：** - POST

**参数：**

========== ==== ====== ================
参数名     必选 类型   说明
========== ==== ====== ================
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
uniqueNo   是   string 用户交易唯一编码
========== ==== ====== ================

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": "",
       "data": {
           "userTransferRecord": {
               "_id": "5ee37114bb788219aed8bfe2",
               "uniqueNo": "1591963924508",
               "fromOwner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "updateTime": 1591963924508,
               "amount": 10,
               "type": 1,
               "status": 0,
               "txHash": "",
               "__v": 0
           },
           "tokenTransferRecord": [{
               "_id": "5ee37114bb788219aed8bfe3",
               "key": 169,
               "uniqueNo": "1591963924513",
               "userTransferNo": "1591963924508",
               "fromKey": 169,
               "toKey": 139,
               "fromOwner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "toOwner": "0x1e236ac89d1ca8bc77eae55fa32ad1189b4b0b5e",
               "keyHash": "f57e5cb1f4532c008183057ecc94283801fcb5afe2d1c190e3dfd38c4da08042",
               "valueHash": "62fa12c6c91b588aa29f07d1cbd6909ac3f843f6dd88f946bc70fe8e686b1ce9",
               "valueProof": "[{\"left\":\"f57e5cb1f4532c008183057ecc94283801fcb5afe2d1c190e3dfd38c4da08042\"}]",
               "updateTime": 1591963924513,
               "txHash": "0xbecbbd2b53d1967c5a5752452429c0cbbc61f0ec8da6b233cf60d684e15a7a76",
               "rootHash": "0x1f9614f8d9af138b854adcf7445e310b36fc9d66f62e17816f496f96336a573e",
               "amount": 10,
               "type": 1,
               "status": 1,
               "owner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "value": "100,0x731d00ede888d9435250545366a29396c5a5c96a&debit,1591963924513,169,139,10,0x58be683c696688829cf199161a3623c009480733f49d06ec38b208179a592f3e4b7da75f94c2ffa8a5b2ae87e68dc95072e22d51180fc804f1b39341d64810c21c",
               "balance": 90,
               "__v": 0,
               "max": 169,
               "min": 169,
               "blockNumber": 78673
           }]
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

============== ====== ========================================
data参数名     类型   说明                                     
============== ====== ========================================
key            number 子账户编号                               
uniqueNo       string 唯一编码                                 
userTransferNo string 用户转账唯一编码                         
fromKey        number 支付key                                  
toKey          number 接收key                                  
fromOwner      string 支付方地址                               
toOwner        string 接收方地址                               
keyHash        string 编号hash                                 
valueHash      string value的hash                              
valueProof     string 提现的proof                              
updateTime     number 转账时间                                 
txHash         string 上链交易hash                             
rootHash       string 上链交易hash                             
amount         number 转账金额                                 
type           number 类型（1-支付 2-收款）                    
status         number 状态（0-等待上链 1-上链成功 2-等待确认） 
owner          string token拥有者                              
value          string value明文                                
balance        number token余额                                
max            number 当前树子钱包最大编号                     
min            number 当前树子钱包最小编号                     
blockNumber    number 区块号                                   
============== ====== ========================================

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

--------------------------------------------------------------------

获取提取详细信息
::::::::::::::::::::::::::::::::::

**简要描述：**

-  获取提取详细信息

**请求URL：** - ``http://localhost:3001/getWithdrawDetail``

**请求方式：** - POST

**参数：**

========== ==== ====== ================
参数名     必选 类型   说明
========== ==== ====== ================
userAddr   是   string 当前用户地址
loginToken 是   string 当前登录凭证
uniqueNo   是   string 用户交易唯一编码
========== ==== ====== ================

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": "",
       "data": {
           "userWithdrawRecord": {
               "_id": "5ee372cdbb788219aed8bfe9",
               "uniqueNo": "1591964365117",
               "fromOwner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "toOwner": "",
               "updateTime": 1591964365140,
               "amount": 10,
               "type": 2,
               "status": 0,
               "txHash": "",
               "__v": 0
           },
           "tokenWithdrawRecord": [{
               "_id": "5ee372cdbb788219aed8bfe8",
               "key": 168,
               "balance": 80,
               "amount": 10,
               "owner": "0x731d00ede888d9435250545366a29396c5a5c96a",
               "updateTime": 1591964373197,
               "uniqueNo": "1591964365128",
               "userTransferNo": "1591964365117",
               "keyHash": "80c3cd40fa35f9088b8741bd8be6153de05f661cfeeb4625ffbf5f4a6c3c02c4",
               "type": 2,
               "status": 1,
               "txHash": "0xdddcc59f95a22ae146181bf8397ee4e7042a51077d1166c2db11aa982c8e0f74",
               "rootHash": "0x4e3543df7e627956c573e78344314a1ca021acc96d08d671d9526d578b148113",
               "blockNumber": 78717,
               "min": 168,
               "max": 168,
               "value": "100,0x731d00ede888d9435250545366a29396c5a5c96a&debit,1591963933374,168,139,10,0x85b50a08348589e09ac7124ff3378ea44edf50bed71f61902bf5c01e735afa3a708026b56526ce7c06827a718861254837f8f4eaa5db8ef4764f56c9460f7dcc1b&withDraw,1591964365128,168,10,0x61085d64e7d6ce6671aea2ddc0499a23ce30c8fab8ad39b6c9fb77d8bbcc794e52c4a8d12232d169f4c32bd935617d9e4b96fb8264d26ac862c97bc127c5f32c1b",
               "valueHash": "d7dbe253865bb517278e82ac0b7aab399dd8ebef289d9193e8c90f1b30455992",
               "__v": 0
           }]
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

============== ====== ========================================
data参数名     类型   说明                                     
============== ====== ========================================
key            number 子账户编号                               
uniqueNo       string 唯一编码                                 
userTransferNo string 用户转账唯一编码                         
fromKey        number 支付key                                  
toKey          number 接收key                                  
fromOwner      string 支付方地址                               
toOwner        string 接收方地址                               
keyHash        string 编号hash                                 
valueHash      string value的hash                              
valueProof     string 提现的proof                              
updateTime     number 转账时间                                 
txHash         string 上链交易hash                             
rootHash       string 上链交易hash                             
amount         number 转账金额                                 
type           number 类型（1-支付 2-收款）                    
status         number 状态（0-等待上链 1-上链成功 2-等待确认） 
owner          string token拥有者                              
value          string value明文                                
balance        number token余额                                
max            number 当前树子钱包最大编号                     
min            number 当前树子钱包最小编号                     
blockNumber    number 区块号                                   
============== ====== ========================================

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

--------------------------------------------------------------------

获取充值交易体
::::::::::::::::::::::::::::::::::


**简要描述：**

-  获取充值交易体（充值前调用）。对于SL20系统充值，需要和区块链交互，此方法返回区块链的标准交易体，下一步需要对这个交易体本地签名后提交交易。

**请求URL：** - ``http://localhost:3001/getChargeTx``

**请求方式：** - POST

**参数：**

========== ==== ====== ===============
参数名     必选 类型   说明
========== ==== ====== ===============
userAddr   是   string 发送人/当前用户
loginToken 是   string 当前登录凭证
amount     是   number 充值金额
========== ==== ====== ===============

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "nonce": "0x2",
           "from": "0x50ea2fa73e0c380d296adab7db21f2ccee91e2e4",
           "gasLimit": "0x0",
           "gasPrice": "0x213f7",
           "to": "0xabab995c2f646e792fd1b81479fbabf058dfaea3",
           "value": "0x64",
           "data": "0x4cddae28",
           "shardingFlag": "0x1",
           "chainId": "0x5e6"
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

+----------------------------------------------------------------------+
| data参数名                                                           |
+======================================================================+
| 客户端无需关注此交易体详细信息，获取后通过chai                       |
| n3SignUtil.signTransaction，拿本地私钥对整个对象做签名即可，参考demo |
+----------------------------------------------------------------------+


--------------------------------------------------------------------

获取提取交易体
::::::::::::::::::::::::::::::::::

**简要描述：**

-  获取提取交易体（提取前调用）。对于SL20系统提取，需要和区块链交互，此方法返回区块链的标准交易体，下一步需要对这个交易体本地签名后提交交易。

**请求URL：** - ``http://localhost:3001/getWithdrawTx``

**请求方式：** - POST

**参数：**

========== ==== ====== ===============
参数名     必选 类型   说明
========== ==== ====== ===============
userAddr   是   string 发送人/当前用户
loginToken 是   string 当前登录凭证
key        是   number 提取人子钱包
amount     是   number 充值金额
========== ==== ====== ===============

**返回示例**

::

   {
       "success": true,
       "statusCode": 200,
       "message": null,
       "data": {
           "nonce": "0x3",
           "from": "0x50ea2fa73e0c380d296adab7db21f2ccee91e2e4",
           "gasLimit": "0x0",
           "gasPrice": "0x29c92",
           "to": "0xabab995c2f646e792fd1b81479fbabf058dfaea3",
           "value": "0x0",
           "data": "0x01f70b3c000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000a400000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000",
           "shardingFlag": "0x1",
           "chainId": "0x5e6"
       }
   }

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

+----------------------------------------------------------------------+
| data参数名                                                           |
+======================================================================+
| 客户端无需关注此交易体详细信息，获取后通过chai                       |
| n3SignUtil.signTransaction，拿本地私钥对整个对象做签名即可，参考demo |
+----------------------------------------------------------------------+




.. |br| raw:: html

    <br/>

|br|


操作模块
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

充值
::::::::::::::::::::::::::::::::::

**简要描述：**

-  充值-本地加签

**请求URL：** - ``http://localhost:3001/charge``

**请求方式：** - POST

**参数：**

====== ==== ====== ==============================
参数名 必选 类型   说明
====== ==== ====== ==============================
signTx 是   string 加签交易体（加签方式参考demo）
====== ==== ====== ==============================

**返回示例**

::

   {"success":true,"statusCode":200,"message":"","data":""}

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额

-----------------------------------------------------------------------------

提现
::::::::::::::::::::::::::::::::::

**简要描述：**

-  提取-本地加签

**请求URL：** - ``http://localhost:3001/withdraw``

**请求方式：** - POST

**参数：**

+-----------------+-------------+--------------------+-----------------+
| 参数名          | 必选        | 类型               | 说明            |
+=================+=============+====================+=================+
| message1        | 是          | string             | 格式：us        |
|                 |             |                    | erAddr,totalAmo |
|                 |             |                    | unt&sign(userAd |
|                 |             |                    | dr,totalAmount) |
|                 |             |                    | ，参考demo      |
+-----------------+-------------+--------------------+-----------------+
| message2        | 是          | string             | 格式：[ { key:  |
|                 |             |                    | 1001, amount:   |
|                 |             |                    | 800, proof:     |
|                 |             |                    | “[]”,signInfo:  |
|                 |             |                    | sign(key,       |
|                 |             |                    | toKey，         |
|                 |             |                    | amount),signTx: |
|                 |             |                    | sign(tx) }, {   |
|                 |             |                    | key: 1002,      |
|                 |             |                    | toKey: 2002,    |
|                 |             |                    | amount:         |
|                 |             |                    | 100,proof:      |
|                 |             |                    | “[]”, signInfo: |
|                 |             |                    | sign(key,       |
|                 |             |                    | toKey，         |
|                 |             |                    | amount),signTx: |
|                 |             |                    | sign(tx) } ]，  |
|                 |             |                    | 参考demo        |
+-----------------+-------------+--------------------+-----------------+

**客户端操作：** 1. 校验：提取金额不能大于账户余额

**返回示例**

::

   {"success":true,"statusCode":200,"message":"","data":""}

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额


-----------------------------------------------------------------------------


转账
::::::::::::::::::::::::::::::::::

**简要描述：**

-  转账-本地加签

**请求URL：** - ``http://localhost:3001/transfer``

**请求方式：** - POST

**参数：**

+-----------------+-------------+--------------------+-----------------+
| 参数名          | 必选        | 类型               | 说明            |
+=================+=============+====================+=================+
| message1        | 是          | string             | 格式：          |
|                 |             |                    | fromOwner,toOwn |
|                 |             |                    | er,totalAmount& |
|                 |             |                    | sign(fromOwner, |
|                 |             |                    | toOwner,        |
|                 |             |                    | totalAmount)    |
|                 |             |                    | ，参考demo      |
+-----------------+-------------+--------------------+-----------------+
| message2        | 是          | string             | 格式：[ { key:  |
|                 |             |                    | 1001, toKey:    |
|                 |             |                    | 2001, amount:   |
|                 |             |                    | 800, signInfo:  |
|                 |             |                    | sign(key,       |
|                 |             |                    | toKey，amount)  |
|                 |             |                    | }, { key: 1002, |
|                 |             |                    | toKey: 2002,    |
|                 |             |                    | amount: 100,    |
|                 |             |                    | signInfo:       |
|                 |             |                    | sign(key,       |
|                 |             |                    | toKey，amount)  |
|                 |             |                    | } ]， 参考demo  |
+-----------------+-------------+--------------------+-----------------+

**客户端操作：** 1. 校验：转账金额不能大于付款账户余额 2.
校验：发送人和接收人地址不能重复

**返回示例**

::

   {"success":true,"statusCode":200,"message":"","data":""}

**返回参数说明**

========== ======= ============
参数名     类型    说明         
========== ======= ============
statusCode number  状态码       
success    boolean true / false 
message    string  异常说明     
data       string  查询信息     
========== ======= ============

**状态码**

服务异常状态码 

- 501：db查询异常 
- 502：db插入或更新异常
- 503：上链交易失败 
- 504：设置缓存失败 
- 505: 获取缓存失败 
- 506：登录失败
- 507：rpc服务调用失败

业务异常状态码 

- 600：解析用户信息失败 
- 601：已经有初始化token
- 602：key不合法 
- 603：余额不足 
- 604：提取合法性校验失败
- 605：当前token拥有者异常 
- 606：验签失败 
- 607：参数异常
- 608：接收人地址不存在 
- 609：接收人无初始化子账户
- 610：当前账户无上链余额