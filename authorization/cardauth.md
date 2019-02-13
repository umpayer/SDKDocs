**建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题**

发起预授权可参考`demo`中的`CardAuthActivity`类

```java
final AuthRequest authRequest = new AuthRequest();
authRequest.orderId = "0101" + dateTimeFormat.format(new Date());
authRequest.amount = 1;//根据自己的需求填写实际金额 单位 分(amount为int类型)
UMPay.getInstance().cardPreAuth(authRequest, new UMAuthPayCallBack() {
	@Override
	public void onReBind(int code, String msg) {
	//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
	}
	@Override
	public void onAuthSuccess(CardAuthResponse authResponse) {
	//预授权成功  返回明确的订单状态
	}
	@Override
	public void onAuthFail(CardAuthResponse authResponse) {
	//预授权失败 返回明确的订单状态
	}
	@Override
	public void onAuthUnKnown(CardAuthResponse authResponse) {
	//订单状态未知  没有明确的状态
	需要发起预授权订单状态查询获取订单的明确状态
	}
	@Override
	public void onProgressUpdate(CardAuthResponse authResponse) {
		if (UMCardCode.START_DOWNLOAD_KEY == authResponse.code) {
		//开始下载秘钥
		} else if (UMCardCode.START_READ_CARD == authResponse.code) {
		//秘钥下载成功，提示用户开始刷卡
		} else if (UMCardCode.START_READ_CARD == authResponse.code) {
		//刷卡成功，请输入密码
		} else if (CardPreAuthCode.SEND_AUTH_REQUEST == authResponse.code) {
		//输入密码成功，发起预授权
		}
	}
});
```

**注意：关闭银行卡界面一定要取消读卡**

```java
UMPay.getInstance().stopSearchCard(new UMCancelCardCallback() {
	@Override
	public void onCancelSuccess() {
	}
	@Override
	public void onCancelFail(String error) {
	}
	@Override
	public void onReBind(int code, String msg) {
	}
});
```


**【请求】**


`AuthRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 金额（单位：分）  |
| orderId  | String  | M  | 订单id (推荐接入方后台生成，保证自己平台唯一,最大长度64位)  |


**【响应】**

`CardAuthResponse`类

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