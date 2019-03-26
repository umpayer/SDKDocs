通过Android Studio编译器获取SHA1
第一步、打开Android Studio的Terminal工具
第二步、输入命令：keytool -v -list -keystore keystore文件路径
第三步、输入Keystore密码

Android Studio
获取build.gradle文件中的ApplicationId作为PackageName；如果没有设置ApplicationId，请以AndroidManifest.xml配置文件的package 属性为准。


![](https://huishangplus.github.io/SDKDocs/images/key4.png)


通过Eclipse编译器获取SHA1
使用 adt 22 以上版本，可以在 eclipse 中直接查看。

Windows：在 eclipse 中打开 Window -> Preferances -> Android -> Build。 Mac：在 eclipse 中打开 Eclipse/ADT->Preferances -> Android -> Build。 在弹出的 Build 对话框中 "SHA1 fingerprint" 中的值即为 Android 签名证书的 Sha1 值，如下图所示

![](https://huishangplus.github.io/SDKDocs/images/key5.png)

使用 keytool（jdk自带工具）获取SHA1
按照如下步骤进行操作：

第一步、运行进入控制台。

![](https://huishangplus.github.io/SDKDocs/images/key6.png)
第二步、在弹出的控制台窗口中输入 cd .android 定位到 .android 文件夹。

![](https://huishangplus.github.io/SDKDocs/images/key7.png)
第三步、继续在控制台输入命令。

debug.keystore：命令为：keytool -list -v -keystore debug.keystore 自定义的 keystore：命令为：keytool -list -v -keystore apk的keystore 如下所示：

![](https://huishangplus.github.io/SDKDocs/images/key8.png)

第四步、提示输入密钥库密码，编译器提供的debug.keystore默认密码是android，自定义签名文件的密码请自行填写。输入密钥后回车（如果没设置密码，可直接回车），此时可在控制台显示的信息中获取SHA1值，如下图所示：


![](https://huishangplus.github.io/SDKDocs/images/key9.png)

说明：keystore 文件为 Android 签名证书文件。

调试版keystore 的默认路径 C:\Users\<用户名>\.android\debug.keystore
