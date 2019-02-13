**对扫码付回调`onPayUnknown`的订单调用此方法来查询订单的最终状态，参照`demo`中的`ScanQueryActivity`类**


```java
ScanPayRequest request = new ScanPayRequest();
request.orderId = orderId;//支付的订单号
request.payType = paytype;//支付方式，支付返回的
request.orderDate = orderDate;//订单时间，如果为空就查询当月交易
UMPay.getInstance().scanQuery(request, new MyQueryCallBack());
/**
 * 查询回调
 */
class MyQueryCallBack implements UMScanQueryCallback {
	@Override
    public void onReBind(int code, String msg) {
//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(ScanPayResponse response) {
        //支付成功，订单最终状态
    }
    @Override
    public void onPayFail(ScanPayResponse response) {
        //支付失败，订单最终状态
    }
    @Override
    public void onPaying(ScanPayResponse response) {
        //支付中，用户可能正在输入密码等问题，可以再次发起查询
    }
    @Override
    public void onQueryError(ScanPayResponse response) {
        //查询失败，推荐接入平台不要修改订单状态，还是未知状态，通过错误信息来判断是否需要继续发起查询
    }
}

```


**【请求】**

`ScanPayRequest`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| orderDate  | String  | O  | 格式：yyyyMMdd例如：20180709 如果为空就查询当月交易  |
| orderId  | String  | M  | 订单id (推荐接入方后台生成，保证自己平台唯一,最大长度32位)  |
| payType  | String  | M  | 支付类型 （固定值 WX、AL、YL）微信，支付宝，银联二维码  |
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