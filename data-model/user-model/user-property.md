# 用户属性

## 用户属性的定义

前面几个章节介绍了GrowingIO的用户模型，在实际的分析场景中，我们发现，单纯的用户ID只能满足简单的用户数量的统计，而客户往往需要的是更加深入的分析维度。例如：

如如何按照某一个用户的属性或者特征来分拆分析用户的行为

据此，GrowingIO提供了用户属性的概念，允许用户通过 API 上传用户相关的属性，并支持在分析工具中使用用户属性来进行更加贴近业务的分析。

## 登录用户属性

当您通过 API 将已经登录系统的用户的登录用户ID上传到GrowingIO的同时，您也可以将您希望用于分析的登录用户属性也一并上传给GrowingIO。

适合作为登录用户属性的包括：

* 在您的业务系统中用户的一些基本信息和分类
* 在您的业务系统中已经明确的用户标签

不适合作为登录用户属性的包括：

* 非常频繁变化的用户状态：如用户当前位置、用户累计在线时长等
* 枚举值非常多的用户属性：如用户手机号、用户mac地址、用户注册时间等

选择合适的登录用户属性可以更好的帮助您完成所需的分析工作，[登录用户变量文档](../../data-definition/user-variable/loginuserid.md)中详细描述了如何进行配置和定义。

## 访问用户属性（微信小程序内测中）

GrowingIO在多年的用户行为分析中发现，很多场景下，我们的客户的应用中，并不会强制要求用户登录，但同时也需要一些用户的维度辅助后续的行为分析，如：

* 微信小程序里的用户，用的账号体系是微信自身的，也不一定会跟客户自己的服务打通。大部分客户会在关键结点的时候才要求用户登录使用。但是，客户需要将微信用户属性如性别发送给GrowingIO用于分析
* 客户期望能把自己基于匿名用户的 A/B 测试标签与 GrowingIO 打通来分析，所以需要上传标签给GrowingIO，从而可以在分析工具中使用该标签进行行为上的对比分析

基于上述场景，GrowingIO目前在微信小程序中推出了访问用户属性的功能，允许用户基于访问用户上传用户属性。其他平台下的访问用户属性的功能将在后续推出，敬请期待。[访问用户变量文档](../../data-definition/user-variable/visituserid.md)中阐述了如何进行配置。

## 上传您的用户属性

### 通过SDK上传用户属性

GrowingIO在所有平台的SDK中都提供了对应的API来实现应用运行时上传用户属性

* [JS SDK](https://growingio.gitbook.io/docs/sdk-integration/web-js-sdk#136)
* [iOS SDK](https://growingio.gitbook.io/docs/sdk-integration/ios-sdk#setpeoplevariable)
* [Android SDK](https://growingio.gitbook.io/docs/sdk-integration/android-sdk#zi-ding-yi-shi-jian-he-bian-liang-api)
* [微信小程序 SDK](https://growingio.gitbook.io/docs/sdk-integration/mina-sdk#zhu-ce-yong-hu-bian-liang)

### 通过用户属性上传API上传用户属性（登录用户属性Only）

GrowingIO提供了一个基于HTTP的 用户属性上传接口，您可以通过调用这个接口来实现从服务器批量上传用户属性。

参考：[用户属性上传API](https://growingio.gitbook.io/docs/api/user-property-upload) 



## 常见问题

1. Web和移动App是否支持设置访问用户属性？

   目前访问用户属性这个功能只在微信小程序中试用，后续会推广到所有平台

2. 什么情况下使用SDK上传用户属性？什么情况下使用 用户属性上传API 上传？

   如果您需要上传的用户属性都可以在服务器端获取，我们推荐您尽量使用 用户属性上传API 上传，从而减少SDK使用的网络流量

