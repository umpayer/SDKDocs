可参考`demo`中的[PreauthDoneActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/PreauthDoneActivity.java)类


```java
PreauthDoneRequest doneRequest = new PreauthDoneRequest();
doneRequest.orderId = orderId;//预授权时的订单号
doneRequest.amount = amount;//预授权完成的金额 单位分(amount为int类型)
doneRequest.account = account;//预授权卡号
doneRequest.authCode = authCode;//预授权成功时返回的授权码
doneRequest.orderDate = orderDate;//发起预授权的日期
UMPay.getInstance().preauthDone(doneRequest, new UMPreauthDoneCallBack() {
	@Override
	public void onReBind(int code, String msg) {
	//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
	}
	@Override
	public void onPaySuccess(PreauthDoneResponse response) {
	//预授权完成成功
	}
	@Override
	public void onPayFail(PreauthDoneResponse response) {
	//预授权完成失败
	}
	@Override
	public void onPayUnknown(PreauthDoneResponse response) {
	//预授权完成状态未知，可调用预授权查询接口查询最终状态
	}
});
```

**【请求】**

`PreauthDoneRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 金额（单位：分）  |
| orderId  | String  | M  | 订单id  |
| orderDate  | String  | M  | 预授权的订单日期  |
| authCode  | String  | M  | 预授权返回的授权码  |


**【响应】**

`PreauthDoneResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| code  | int  | M  | 返回码  |
| message  | String  | M  | 响应信息  |
| orderId  | String  | C  | 订单id  |
| amount  | String  | C  | 金额（单位：分）  |
| cardPayType  | String  | C  | 银行卡类型，包括磁条卡（CT），IC卡（IC），云闪付、applepay（YS）,银行卡闪付（YSIC）  |
| payType  | String  | C  | 银行卡类型，包括磁条卡（CT），IC卡（IC），闪付卡、云闪付、applepay（YS）  |
| tradeNo  | String  | C  | 平台处理流水  |
| orderDate  | String  | C  | 订单日期（退款的时候使用） 格式：yyyyMMdd  |
| platTime  | String  | C  | 平台时间HHmmss  |
| paySeq  | String  | C  | 参考号（打印小票必要字段）  |
| unionPayMerId  | String  | C  | 商户编号，商户在银联注册的商户号  |
| unionPayPosId  | String  |  C | 终端编号  |
| uMerId  | String  | C  | U付商户号  |
| uPosId  | String  | C  | U付终端号  |
| bankName  | String  | C  | 发卡行  |
| account  | String  | C  | 明文卡号  |
| cardExpiryDate  | String  | C  | 卡有效期  |
| uPayTrace  | String  | C  | 凭证号  |
| batchId  | String  | C  | 批次号  |
| authCode  | String  | C  | 授权码  |