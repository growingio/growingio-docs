# 微信小程序二维码创建服务 API

## 说明

微信小程序二位码，是调用腾讯微信小程序接口的。其中创建小程序码，腾讯提供 A、B 两个接口；创建二维码，微信提供 C 接口。

GrowingIO 在授权后，调用的是 A 和 C 接口创建小程序码和二维码。

具体详情请见微信开发者文档：[https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/qr-code.html](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/qr-code.html)

其中

{% hint style="warning" %}


1. 接口只能生成**已发布**的小程序的码；
2. 接口 A 加上接口 C，总共生成的码数量限制为 100,000；如果使用 GrowingIO 的批量创建码的服务，**请谨慎调用**。
{% endhint %}

## 接口调用文档

|  |  |  |
| :--- | :--- | :--- |
|  |  |  |

