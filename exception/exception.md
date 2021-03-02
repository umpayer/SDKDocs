#### 1.应用被占用

> 执行刷卡、打印时，SDK会限制客户端调用，确保流程完整和唯一,多次调用刷卡或者打印功能时会提示应用被占用，如果出现一直无法解除占用的异常情况，此时可以调用如下代码，解除之前的异常占用，可不理会回调的结果信息

```java
UMFintech.getInstance().getPosInfo(new PosInfoCallBack() {
  @Override  
  public void onSuccess(PosInfo info) {
		///获取到的信息
  }
  @Override
  public void onError(String info) {

  }
  @Override
  public void onReBind(int code, String msg) {                 
  }  
});
```

