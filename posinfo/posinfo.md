#### 1.获取Pos终端信息

获取终端信息可以参考`demo`中的[GetSnActivity]()类。

```java
UMFintech.getInstance().getPosInfo(new PosInfoCallBack() {
  @Override  
  public void onSuccess(PosInfo info) {
		///获取到的信息
  }
  @Override
  public void onError(String info) {
    tv_tusn.setText(info); 
  }
  @Override
  public void onReBind(int code, String msg) {                 
  }  
});
```

**【响应】**


| 字段    | 类型   | 必须 | 描述        |
| :------ | ------ | ---- | ----------- |
| message | String | M    | 响应信息    |
| code    | int    | M    | 返回码      |
| possn   | String | O    | 设备的possn |
| posname | String | O    | 设备的名称  |
| tusn    | String | O    | 设备的tusn  |

