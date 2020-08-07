建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题

扫码付支持微信、支付宝、银联二维码，可以参考demo中的`ScanInitiativeActivity`类

#### 请求收款码展示给客户扫码

```java
ScanPayInitiativeRequest scanPayInitiativeRequest = new ScanPayInitiativeRequest();
scanPayInitiativeRequest.amount = Integer.parseInt(amount);
scanPayInitiativeRequest.tips = "定金"; //tips不传默认为 消费
scanPayInitiativeRequest.payType = payType;
scanPayInitiativeRequest.orderId = orderId;
UMFintech.getInstance().scanPayInitiative(scanPayInitiativeRequest, umScanPayInitiativeCallback);
```

**`UMScanPayInitiativeCallback`回调**

```java
private UMScanPayInitiativeCallback umScanPayInitiativeCallback = new UMScanPayInitiativeCallback() {
        @Override
        public void onReBind(int code, String msg) {
            //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
        }

        @Override
        public void onGetQRCodeSuccess(ScanPayInitiativeResponse response) {
            //请求二维码成功
        }

        @Override
        public void onGetQRCodeFail(ScanPayInitiativeResponse response) {
            //请求二维码失败
        }

};

```

**`ScanPayInitiativeRequest`类【请求】**

| 字段    | 类型   | 必须 | 描述                                                         |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| amount  | int    | M    | 金额（单位：分）                                             |
| tips    | String | O    | 支付说明，默认为消费                                         |
| payType | String | M    | 支付类型(WX、AL、YL分别对应：微信，支付宝，银联)             |
| orderId | String | O    | 商户订单号。建议商户平台自己维护，  生成规则：商户平台定义：<br/>小于64位的非空字符  插件定义：yyMMddHHmmssSSS+{postusn} |

**`ScanPayInitiativeResponse`类【响应】**

| 字段      | 类型   | 必须 | 描述                                           |
| --------- | ------ | ---- | ---------------------------------------------- |
| message   | String | M    | 响应信息                                       |
| code      | int    | M    | 返回码                                         |
| uPayTrace | String | O    | 支付凭证号                                     |
| qrcodeUrl | String | O    | 收款码，通过此信息生成                         |
| payType   | String | O    | 支付类型（WX、AL、YL）微信，支付宝，银联二维码 |
| date      | String | O    | 平台日期  格式：MMdd                           |
| amount    | String | O    | 订单金额（单位：分）                           |
| orderId   | String | O    | 订单号                                         |
| time      | String | O    | 平台时间HHmmss                                 |
| stateCode | String | O    | 后台返回的状态码                               |
| msgToUser | String | O    | 错误用户提示                                   |
| msgDesc   | String | O    | 解释口径                                       |



#### 请求查询客户付款状态

```java
ScanPayStateQueryRequest request = new ScanPayStateQueryRequest();
request.amount = Integer.parseInt(scanPayInitiativeResponse.amount);
request.date = scanPayInitiativeResponse.date;
request.time = scanPayInitiativeResponse.time;
request.upayTrace = scanPayInitiativeResponse.uPayTrace;
request.payType = scanPayInitiativeRequest.payType;
request.orderId = scanPayInitiativeResponse.orderId;
//request.interval = 8; //8秒轮询一次 （根据用户需求自定义，不传默认为8）
//request.repeatCount = 5; //总共轮询5次（根据用户需求自定义，不传默认为5）
//注意：interval和repeatCount参数只是为了提供定制化需求，即通过这两个参数配置后台轮询次数和间隔，可以配置 repeatCount = 1（即该请求只轮询查询一次）， 如果回调走 onPayUnknown方法，接入方可以再次调用查询支付状态，以防止出现后台长时间轮询请求，而Activity已经处于销毁状态的情况
UMFintech.getInstance().scanPayStateQuery(request, umScanPayStateQueryCallback);
```

**`UMScanPayStateQueryCallback`回调**

```java
private UMScanPayStateQueryCallback umScanPayStateQueryCallback = new UMScanPayStateQueryCallback() {
    @Override
    public void onReBind(int code, String msg) {
	    //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }

    @Override
    public void onPaySuccess(ScanPayStateQueryResponse response) {
        //支付成功
    }

    @Override
    public void onPayFail(ScanPayStateQueryResponse response) {
        //支付失败
    }

    @Override
    public void onPayUnknown(ScanPayStateQueryResponse response) {
        //支付状态未知 (在用户配置的轮询次数和间隔之后还是没查到支付状态)
		//可继续发起查询请求
    }

};
```

**`ScanPayStateQueryRequest`【请求】**

| 字段        | 类型   | 必须 | 描述                                             |
| ----------- | ------ | ---- | ------------------------------------------------ |
| amount      | int    | M    | 金额（单位：分）                                 |
| upayTrace   | String | M    | 支付凭证号                                       |
| payType     | String | M    | 支付类型(WX、AL、YL分别对应：微信，支付宝，银联) |
| date        | String | M    | 平台日期  格式：MMdd                             |
| time        | String | M    | 平台时间HHmmss                                   |
| repeatCount | int    | O    | 该请求需要轮询的总次数，默认5次                  |
| interval    | int    | O    | 轮询时间间隔，单位秒，默认8秒                    |
| orderId     | String | M    | 订单id                                           |

**`ScanPayStateQueryResponse`【响应】**

| 字段        | 类型   | 必须 | 描述                                           |
| ----------- | ------ | ---- | ---------------------------------------------- |
| message     | String | M    | 响应信息                                       |
| code        | int    | M    | 返回码                                         |
| uPayTrace   | String | O    | 支付凭证号                                     |
| payType     | String | O    | 支付类型（WX、AL、YL）微信，支付宝，银联二维码 |
| date        | String | O    | 平台日期  格式：MMdd                           |
| payAmount   | String | O    | 支付金额（单位：分）                           |
| orderId     | String | O    | 订单号                                         |
| time        | String | O    | 平台时间HHmmss                                 |
| uPayOrderId | String | O    | U付订单号                                      |
| stateCode   | String | O    | 后台返回的状态码                               |
| msgToUser   | String | O    | 错误用户提示                                   |
| msgDesc     | String | O    | 解释口径                                       |

