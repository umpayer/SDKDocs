# 联动服务商SDK接入文档

## 文档说明
本文为接入联动服务商交易的接口标准。
读者对象为联动服务商的产品设计、开发和测试人员以及第三方接入平台或商户。

## 修改记录
| 修改日期  |  修改内容 | 版本号  | 修改人  |
| :---- | :----------- | :-- | :---- |
| 2017.06.01  | 初稿  |  V0.1 |  田晓阳、王凯 |
| 2017.06.29  | 1.对扫码付和银行卡支付的orderId字段着重说明<br/>2.对扫码付的mediaNo字段描述进行了修改  |  V0.2 | 田晓阳  |
| 2017.09.13  |  1.删除获取卡号和获取商户信息接口<br/>2.添加获取磁条卡磁道信息接口<br/>3.扫码付添加支持银联二维码支付、查询、退款 | V0.3  | 田晓阳  |
| 2017.10.17  | 1.所有接口方法添加重新绑定插件回调方法<br/>2.银行卡支付添加小额双免功能，响应添加freeAmt、freeMessage字段  | V0.4  | 田晓阳  |
| 2017.12.08  | 1.添加新的打印方法<br/>2.添加获取商户信息接口<br/>3.银行支付成功响应添加authCode字段  |  V0.5 | 王凯、田晓阳  |
| 2017.12.21  | 1.添加银行卡预授权，预授权撤销，预授权完成有卡，<br />预授权完成无卡，预授权撤销，预授权订单状态查询6个接口 | V0.6  | 王凯  |
| 2018.02.27  | 1.所有请求的amount字段类型 改成int  | V0.7  | 段晓娜  |
| 2018.03.01  | 1.添加说明所有方法运行在主线程中<br/>2.添加建议  交易数据存在接入方后台，便于排查问题  | V0.8  | 段晓娜  |
| 2018.03.09  | 1.添加获取厂商名称和SN的方法  | V0.9  | 段晓娜  |
| 2018.03.12  | 1.添加goodsInfo,goodsDescribe 的字段限制说明  | V1.0  | 段晓娜  |
| 2018.03.27  | 1.添加SDK混淆配置  | V1.1  | 田晓阳  |
| 2018.05.15  | 1.删除获取磁条卡磁道信息接口  | V1.2  | 田晓阳  |
| 2018.07.09  | 1.扫码付接口响应字段paySeq修改解释<br/>2.扫码付订单状态查询接口添加 orderDate字段  | V1.3  | 田晓阳  |
| 2018.08.01  | 1.银行卡支付和银行卡订单状态查询接口响应添加<br />cardType字段 | V1.4  | 田晓阳  |
| 2018.08.20  | 1.添加读取会员卡的方法<br/>2.添加蓝牙连接方法  | V1.5  | 段晓娜  |
| 2018.09.20  | 1.银行卡支付添加新返回字段cardPayType<br/>2.修改银行卡撤销 和退款传入的类型<br/>3.银行卡订单状态查询添加cardPayType  | V1.6  | 王凯  |
| 2019.09.01  | 1.修改支付模式，接口切换为直连<br/>2.修改POS机交互流程，重新适配联迪A8  | V00.00.01.00  | 许岩  |
| 2019.11.01  | 1.适配天喻P20  | V00.00.01.01  | 周四川  |
| 2020.03.20  | 1.适配商米P1/P2  | V00.00.01.07  | 许岩  |
| 2020.04.03  | 1.适配升腾K9  | V00.00.01.10  | 周四川  |
| 2020.07.25  | 1.交易失败时，新增返回字段msgDesc、msgToUse、stateCode  | V00.00.01.30  | 周四川  |
| 2020.08.01  | 1.银行卡支付新增读卡流程回调  | V00.00.01.31  | 许岩  |
| 2020.08.03  | 1.银行卡撤销和退款新增读卡流程回调  | V00.00.01.32  | 许岩  |
| 2020.08.06  | 1.新增交易中失败返回码。  | V00.00.01.33  | 许岩  |
| 2020.11.24 | 1.增加获取POS信息。 | V00.00.01.36 | 许岩 |
| 2020.12.22 | 1、增加商户信息获取接口<br />2、修复天喻设备打印二维码失败的异常 | V00.00.01.39 | 许岩 |

