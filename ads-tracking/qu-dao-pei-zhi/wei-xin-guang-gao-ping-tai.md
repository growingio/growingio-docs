# 微信广告平台

1、登录微信广告投放平台\(mp.qq.weixin.com\)，找到推广模块，进入广告主菜单。

2、在账户管理菜单，点击【获取移动应用激活数据】，进入对应功能页面。

![](blob:https://app.gitbook.com/2a8fbb0a-ce63-43b3-bbbf-1f0931f6294d)

3、点击【+绑定移动应用】按钮，开始创建转化规则，可以选择 iOS 或者 Android 应用类型。

![](blob:https://app.gitbook.com/04390da6-278b-45d6-8059-0959b6a3266b)



4、填写需要创建数据转化规则的 Apple ID 校验，选择转化类别，并在 Feedback URL 中填写从 GIO 中获取的监测链接。

![](blob:https://app.gitbook.com/759fb0e1-ccbc-4b4f-86c4-7dd8fb18e3c6)

![](../../.gitbook/assets/image%20%284%29.png)

{% hint style="info" %}
目前 GrowingIO 在微信 mp 渠道下支持【激活】类型事件回传。
{% endhint %}

5、Feedback URL 填写完成后，获取密钥 **encrypt\_key** 及 **sign\_key**， 并回填至 GIO 。

![](blob:https://app.gitbook.com/109cb082-9bd8-4733-88b9-d5e27f0cd60b)

6、回填完成后，点击【确定】即完成该应的转化回传规则配置。

7、配置完成后需要在微信广告平台进行联调，联调成功后需启用后才能正式生效。

