
**建议：** 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题

扫码付支持微信、支付宝、银联二维码，可以参考`demo`中的`ScanActivity`类


```java
ScanPayRequest request = new ScanPayRequest();
request.amount = amount;//amount为int类型
request.mediaNo = result.getText();
request.goodsDescribe = "商品描述商品描述";
request.goodsInfo = "商品信息"
//订单id (推荐接入方后台生成，保证自己平台唯一,最大长度32位)，下方为测试所用
request.orderId = "0101" + dateTimeFormat.format(new Date());
UMPay.getInstance().scanPay(request, umScanPayCallback);
```

`UmScanPayCallback`回调

```java
private UMScanPayCallback umScanPayCallback = new UMScanPayCallback() {
	@Override
    public void onReBind(int code, String msg) {
//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(ScanPayResponse response) {
        //支付成功回调，最终订单状态
    }
    @Override
    public void onPayFail(ScanPayResponse response) {
         //支付失败回调，最终订单状态
    }
    @Override
    public void onPayUnknown(ScanPayResponse response) {
        //支付状态未知回调（由于网络和各种异常有可能会出现这种状态），不能确定订单最终状态，推荐接入平台记录状态为未知，后续可以再次调用扫码付状态查询方法，来确定最终状态
	//UMPay.getInstance().scanQuery();
    }
};

```

**【请求】**

`ScanPayRequest`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| amount  | int  | M  | 金额（单位：分）  |
| mediaNo  | String  | M  | 二维码编号（微信，支付宝，银联二维码）  |
| goodsInfo  | String  | M  | 商品信息(不能超过6个字)  |
| goodsDescribe  | String  | M  | 商品描述（不能超过42个字）  |
| orderId  | String  | M  | 订单id (推荐接入方后台生成，保证自己平台唯一,最大长度32位)  |
| reserve  | String  | O  | 保留字段  |



**【响应】**

`ScanPayResponse`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | C  | 订单号  |
| payType  | String  | C  | 支付类型（WX、AL、YL）微信，支付宝，银联二维码  |
| paySeq  | String  | C  | 微信、支付宝、银联二维码返回的支付流水号  |
| payDay  | String  | C  | 支付日期 格式：yyyyMMdd  |
| amount  | String  | C  | 订单金额（单位：分）  |
| coupAmount  | String  | C  | 优惠金额（单位：分）  |
| payAmount  | String  | C  | 实际支付金额（单位：分）  |
| tradeNo  | String  | C  | 平台处理流水  |
| orderDate  | String  | C  | 订单日期 格式：yyyyMMdd  |
| platTime  | String  | C  | 平台时间HHmmss  |
| buyerLogonId  | String  | O  | 买家支付宝账号  |
| buyerId  | String  | O  | 买家支付宝用户UID  |
| openId  | String  | O  | 微信用户标识  |
| subOpenId  | String  | O  | 微信用户子标识  |
| bankType  | String  | O  | 银行类型  |
| promotionDetail  | String  | O  | 优惠信息  |



