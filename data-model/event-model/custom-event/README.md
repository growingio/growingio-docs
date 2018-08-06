# 埋点事件

## 埋点事件的定义

在很多业务分析场景中，单纯的无埋点事件无法满足用户的分析需求

与其他数据分析产品一样，GrowingIO在 SDK 中也为用户提供了自定义事件的 API

这些通过自定义事件 API 发送到GrowingIO的事件，我们称为埋点事件

在GrowingIO系统中，埋点事件的行为层级关系等同于无埋点事件的动作事件

## 埋点事件的分类

按照埋点事件的发送位置不同，埋点事件可以分为 客户端埋点和服务端埋点两大类：

### 客户端埋点

客户可以通过在自己的应用中通过调用 SDK 的 API 来发送埋点事件，对应的 API 文档参考：

* [WEB JS](https://growingio.gitbook.io/docs/sdk-integration/web-js-sdk#track)
* [iOS](https://growingio.gitbook.io/docs/sdk-integration/ios-sdk#track)
* [Android](https://growingio.gitbook.io/docs/sdk-integration/android-sdk#zi-ding-yi-shi-jian-he-bian-liang-api)
* [小程序](https://growingio.gitbook.io/docs/sdk-integration/mina-sdk#zi-ding-yi-shi-jian-pei-zhi)

### 服务端埋点

对于一些复杂的业务事件，如：

* 应用的客户端没有明确的操作成功标记，无法通过无埋点事件进行分析
* 线下营销活动数据录入，无法通过无埋点事件进行分析

客户可以在服务端通过调用 GrowingIO 的服务器埋点事件接口来发送埋点事件。

该功能还在内测阶段中，请联系您的技术支持或客户经理

