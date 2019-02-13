**对银行卡支付回调`onPayUnknown`的订单调用此方法来查询订单的最终状态，参照`demo`中的`CardPayQueryActivity`类**


```java
CardPayQueryRequest request = new CardPayQueryRequest();
request.orderId = orderId.getText().toString().trim();//支付的订单号
request.orderDate = orderDate.getText().toString().trim();//订单时间 格式yyyyMMdd （由于数据库做的分表，所以需要传这个字段，如果不传就查询当月数据）
UMPay.getInstance().cardQuery(request,new UMCardPayQueryCallback(){
	@Override
    public void onReBind(int code, String msg) {
 //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(BankCardPayResponse response) {
       //支付成功，订单最终状态
    }
    @Override
    public void onPayFail(BankCardPayResponse response) {
        //支付失败，订单最终状态
    }
    @Override
    public void onPaying(BankCardPayResponse response) {
        //支付中，也就是未支付的
    }
    @Override
    public void onQueryError(BankCardPayResponse response) {
        //查询失败，推荐接入平台不要修改订单状态，还是未知状态，通过错误信息来判断是否需要继续发起查询    }
});

```

**【请求】**

`CardPayQueryRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| orderId  | String  | M  | 订单号  |
| orderDate  | String  | M  | 订单时间 格式yyyyMMdd （由于数据库做的分表，所以需要传这个字段，如果不传就查当月数据）  |


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
