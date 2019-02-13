**非当天的做的交易想还原交易需要调用这个接口，参照`demo`中`ScanRefundActivity`类
所需要的字段都是通过支付返回的字段**


```java
RefundRequest request = new RefundRequest();
request.refundPartnerOrderId = refundPartnerOrderId;
request.orderDate = orderDate;
request.orderId = orderId;
request.amount = amount;(amount为int类型)
request.payType = payType;
UMPay.getInstance().scanRefund (request, new UMScanRefundCallback () {
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
	//UMPay.getInstance().scanQuery ()
    }
});

```
