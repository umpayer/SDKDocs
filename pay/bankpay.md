**建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题**

银行卡支付支持`磁条卡`、`IC卡`、`闪付卡`、`applepay`、`云闪付等`可以参考`demo`中的`CardActivity`类。
```java
BankCardPayRequest payRequest = new BankCardPayRequest();
payRequest.amount = amount;//amount为int类型
//订单id (推荐接入方后台生成，保证自己平台唯一,最大长度32位)，下方为测试所用
payRequest.orderId = "0101" + dateTimeFormat.format(new Date());
UMPay.getInstance().cardPay(payRequest, new UMBankCardPayCallback() {
	@Override
    public void onReBind(int code, String msg) {
//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(BankCardPayResponse response) {
        //支付成功回调，最终订单状态
    }
    @Override
    public void onPayFail(BankCardPayResponse response) {
       //支付失败回调，最终订单状态
    }
    @Override
    public void onPayUnknown(BankCardPayResponse response) {
    //支付状态未知回调（由于网络和各种异常有可能会出现这种状态），不能确定订单最终状态，推荐接入平台记录状态为未知，后续可以再次调用银行卡支付状态查询方法，来确定最终状态
//UMPay.getInstance().cardQuery();
    }

    @Override
    public void onProgressUpdate(BankCardPayResponse response) {
        //各种流程回调
        if(UMCardCode.START_DOWNLOAD_KEY ==  response.code){
            //开始下载密钥（做银行卡支付需要先下载密钥）
        }else if(UMCardCode.START_READ_CARD ==  response.code){
            //密钥下载成功，开始刷卡！
        }else if(UMCardCode.START_READ_CARD ==  response.code){
            //刷卡成功，请输入密码
        }else if(UMCardCode.SEND_PAY_REQUEST ==  response.code){
            //密码输入成功，发起支付
        }
    }
});

```

**注意：关闭银行卡界面一定要取消读卡**

```java
UMPay.getInstance().stopSearchCard(null);
```


**【请求】**

`BankCardPayRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 金额（单位：分） |
| orderId  | String  | M  | 订单id (推荐接入方后台生成，保证自己平台唯一,最大长度32位)  |
| reserve  | String  | O  | 保留字段  |



**【响应】**

`BankCardPayResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
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
| account  | String  | C  | 脱敏卡号  |
| cardExpiryDate  | String  | C  | 卡有效期  |
| uPayTrace  | String  | C  | 凭证号  |
| batchId  | String  | C  | 批次号  |
| authCode  | String  | C  | 授权码  |
| freeAmt  | String  | C  | 小额双免金额，0不是小额双免  |
| freeMessage  | String  | C  | 小额双免提示语，打印小票使用，例如：交易金额未超[1000.00]元，免密免签  |
| cardType  | String  | C  | 卡类型，00借记卡 01贷记卡  |



