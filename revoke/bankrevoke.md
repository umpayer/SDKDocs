**当天的做的交易想还原交易需要调用这个接口，参照`demo`中`BankRevokeActivity`类
所需要的字段都是通过支付返回的字段**

```java
RefundRequest request = new RefundRequest();
request.orderDate = orderDate;
request.orderId = orderId;
request.amount = amount;
request.payType = payType;
UMPay.getInstance().bankRevoke (request, new UMRefundCallback () {
	@Override
	public void onReBind(int code, String msg) {
	 //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
	}
    @Override
    public void onRefundSuccess(RefundResponse response) {
       //退款成功，订单最终状态
    }
    @Override
    public void onRefundFail(RefundResponse response) {
        //退款失败，订单最终状态
    }
    @Override
    public void onRefundUnknown(RefundResponse response) {
        //退款未知，需要再起发起退款或调用退款状态查询接口
	//UMPay.getInstance().cardQuery ()
    }
	@Override
    public void onProgressUpdate(RefundResponse response) {
        //各种流程回调
        if(UMRefundCode.START_DOWNLOAD_KEY ==  response.code){
            //开始下载密钥
        }else if(UMRefundCode.START_READ_CARD ==  response.code){
            //密钥下载成功，开始刷卡！
        }else if(UMRefundCode.START_READ_CARD ==  response.code){
            //刷卡成功，请输入密码
        }else if(UMRefundCode.SEND_PAY_REQUEST ==  response.code){
            //密码输入成功，发起退款
        }
    }
});

```


**【请求】**

`RefundRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 退款金额 分，交易成功之后返回的amount  |
| tradeNo  | String  |  O | 流水号，交易成功之后返回的  |
| orderId  | String  | M  | 订单号，交易成功之后返回的  |
| orderDate  | String  | M  | 订单日期，交易成功之后返回的 格式：yyyyMMdd  |
| payType  | String  | M  | 支付类型（CT、IC、YS、YSIC、WX、ZFB、YL）磁条卡、ic卡、闪付卡、微信、支付宝、银联二维码交易成功之后返回的  |


**【响应】**

`RefundResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | C  | 订单id  |
| amount  | String  | C  | 金额（单位：分）  |
| tradeNo  | String  | C  | 平台处理流水  |
| platDate  | String  | C  | 平台日期 格式：yyyyMMdd  |
| platTime  | String  | C  | 平台时间HHmmss  |
| uPospDate  | String  | C  | POSP日期 ,退款状态查询不会返回此此段  |
| uPospTime  | String  |  C | POSP时间 ,退款状态查询不会返回此此段  |
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
