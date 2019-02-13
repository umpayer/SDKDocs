**用来查询退款状态未知的情况下来查询退款是否成功，参考`demo`中的[RefundStateQueryActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/RefundStateQueryActivity.java)**


```java
RefundQueryRequest request = new RefundQueryRequest();
request.orderId = orderId.getText().toString().trim();
//订单时间 格式yyyyMMdd （由于数据库做的分表，所以需要传这个字段，如果不传就查当月数据）
request.orderDate = orderDate.getText().toString().trim();
UMPay.getInstance().refundQuery(request,new UMRefundStateQueryCallback(){
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
    public void onQueryError(RefundResponse response) {
        //退款未知，通过错误信息来判断是否需要继续发起退款或调用退款状态查询接口
    }
});

```

**【请求】**

`RefundQueryRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| orderId  | String  | M  | 订单号  |
| orderDate  | String  | M  | 订单时间 格式yyyyMMdd （由于数据库做的分表，所以需要传这个字段，如果不传就查当月数据）  |


**【响应】**

`RefundResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | C  | 订单id  |
| amount  | String  | C  | 退款金额（单位：分）  |
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


