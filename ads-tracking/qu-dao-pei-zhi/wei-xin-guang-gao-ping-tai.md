# 微信广告平台

1、登录微信广告投放平台\(mp.qq.weixin.com\)，找到推广模块，进入广告主菜单。

![](blob:https://app.gitbook.com/977c318f-4cfa-4168-9264-cc8c7ab677cc)

2、在账户管理菜单，点击【获取移动应用激活数据】，进入对应功能页面。

![](blob:https://app.gitbook.com/f2132409-44d7-44bb-ab49-25775a0db4b2)

3、点击【+绑定移动应用】按钮，开始创建转化规则，可以选择 iOS 或者 Android 应用类型。

{% hint style="info" %}
目前 GrowingIO 在微信 mp 渠道下支持【激活】类型事件回传。
{% endhint %}

![page7image34590576](blob:https://app.gitbook.com/bf244004-1665-4f4a-8503-0612e7b23ae3)

4、填写需要创建数据转化规则的 Apple ID 校验，选择转化类别，并在 Feedback URL 中填写从 GIO 中获取的监测链接。

![](../../.gitbook/assets/image%20%284%29.png)

5、Feedback URL 填写完成后，获取密钥 **encrypt\_key** 及 **sign\_key**， 并回填至 GIO 。

![page8image34374256](blob:https://app.gitbook.com/c43b8143-5ca8-496e-8021-09c7c36272bc)

6、回填完成后，点击确认即完成该应的转化回传规则配置。

