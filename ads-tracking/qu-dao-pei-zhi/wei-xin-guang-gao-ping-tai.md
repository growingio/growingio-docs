# 微信广告平台

1、登录微信广告投放平台\(mp.qq.weixin.com\)，找到推广模块，进入广告主菜单。

2、在账户管理菜单，点击【获取移动应用激活数据】，进入对应功能页面。

![](../../.gitbook/assets/image%20%28437%29.png)

3、点击【+绑定移动应用】按钮，开始创建转化规则，可以选择 iOS 或者 Android 应用类型。

![](../../.gitbook/assets/image%20%28321%29.png)

4、填写需要创建数据转化规则的 Apple ID 校验，选择转化类别，并在 Feedback URL 中填写从 GIO 中获取的监测链接。

![](../../.gitbook/assets/image%20%2886%29.png)

![](../../.gitbook/assets/image%20%284%29.png)

![](../../.gitbook/assets/image%20%28225%29.png)

{% hint style="info" %}
目前 GrowingIO 在微信 mp 渠道下支持【激活】类型事件回传。
{% endhint %}

5、Feedback URL 填写完成后，获取密钥 **encrypt\_key** 及 **sign\_key**， 并回填至 GIO 。

![](../../.gitbook/assets/image%20%28113%29.png)

6、回填完成后，点击【确定】即完成该应的转化回传规则配置。

7、配置完成后需要在微信广告平台进行联调，联调成功后需启用后才能正式生效。

