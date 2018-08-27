---
description: 欢迎使用 GrowingIO，这是一份新同学使用 GrowingIO 指南
---

# 快速上手

我们为你准备了强大的数据采集和分析工具，从这里开始你的数据分析之旅：

### 第一步：与工程师一起接入 SDK

1. [JS SDK 配置](sdk-integration/web-js-sdk.md)
2. [Android SDK 配置](sdk-integration/android-sdk/)
3. [iOS SDK 配置](sdk-integration/ios-sdk.md)
4. [**小程序 SDK 配置**](sdk-integration/mina-sdk.md)

### 第二步：在工程师的协助下，进行重要配置

1. 如果你是使用了 \#hashtag 的单页面应用，请[进行配置](sdk-integration/web-js-sdk.md#21-hashtag-xi-tong-bian-liang)
2. 95% 的用户都会上传用户属性字段，这样你就可以知道产品内的行为是由具体的哪个用户做的。比如想要对比登录和未登录用户的差异，就需要[上传登录用户 ID ](sdk-integration/web-js-sdk.md#137)。

### 第三步：选择数据定义方式

GrowingIO 提供了两种数据采集定义方式「无埋点 - 圈选」和「埋点 - 自定义事件和变量」：

1. 通过 [web 圈选](data-definition/circle/web.md) 、[iOS / Android 移动端圈选](data-definition/circle/app.md)、[小程序圈选](data-definition/circle/minp.md)进行数据定义。
2. 通过埋点创建[自定义事件和变量](data-model/event-model/custom-event/)
3. 了解 [GrowingIO 数据模型](data-model/) ，以及产品内[预定义指标和维度](data-model/olap-model/predifined-metrics-dimensions.md)的基本解释。

### 第四步：使用数据分析工具

1. 事件分析 [文档](data-analytics/event-analysis.md) \| [使用手册](https://s.growingio.com/nvN9MB)
2. 漏斗分析 [文档](data-analytics/funnel-analysis.md) \| [使用手册](https://s.growingio.com/9PXbR0) \| [视频](https://s.growingio.com/kKdDjv)
3. 留存分析 [文档](data-analytics/retention-analysis.md) \| [使用手册](https://s.growingio.com/p8QD3x) \| [视频](https://s.growingio.com/4PpoAK)
4. 用户分群 [文档](data-analytics/user-segmentation.md) \| [使用手册](https://s.growingio.com/9PaAZ8) \|  [视频](https://s.growingio.com/ambRb4)
5. 广告监测 [文档](ads-tracking/) \| [视频1](https://s.growingio.com/DmQMzB) \| [视频2](https://s.growingio.com/KqZEP3) \| [视频3](https://s.growingio.com/jvoRdB)
6. 智能路径 [文档](data-analytics/pathfinder.md)
7. 留存魔法师 [文档](data-analytics/magic-number.md)
8. 热图 [文档](data-analytics/heatmap/)
9. 实时分析 [文档](dashboard/realtime.md)
10. 概览分析 [文档](dashboard/overview.md)
11. 小程序概览 [文档](dashboard/mina-overview.md)
12. web 圈选 [文档](data-definition/circle/web.md) \| [使用手册](http://growing.cn-bj.ufileos.com/web_circle.pdf)
13. App 端圈选 [文档](data-definition/circle/app.md) \| [使用手册](http://growing.cn-bj.ufileos.com/app_circle.pdf)
14. **小程序圈选** [文档](data-definition/circle/minp.md) 
15. Deep-Link [文档](https://docs.growingio.com/docs/ads-tracking/app-marketing#113) \| [使用手册](https://s.growingio.com/xzAqPp)

### 第五步：开始进行分析

我们提供了一些主要功能的产品使用手册，希望可以为你带来帮助：

1. [指标和维度使用手册](https://s.growingio.com/NLdx0O)
2. [事件分析使用手册](https://s.growingio.com/nvN9MB)
3. [漏斗分析使用手册](https://s.growingio.com/9PXbR0)
4. [留存分析使用手册](https://s.growingio.com/p8QD3x)
5. [用户分群使用手册](https://s.growingio.com/9PaAZ8)

### 小程序工具快速上手

GrowingIO 所有分析能力都同样支持小程序，同时，还对小程序提供了更有针对性的支持：

1.集成[小程序 SDK ](sdk-integration/mina-sdk.md)，以进行数据采集；利用[数据验证](sdk-integration/growingio-debugger/#growingio-minidebugger)来实时查看采集数据的情况；

2.通过[小程序圈选](data-definition/circle/minp.md)来进行数据定义，同时支持[埋点事件和变量](data-model/event-model/custom-event/)；

3.通过[小程序概览](dashboard/mina-overview.md)了解小程序的基本数据情况；

4.通过 [事件分析](data-analytics/event-analysis.md)、[漏斗分析](data-analytics/funnel-analysis.md)、[留存分析](data-analytics/retention-analysis.md)等功能，来进行产品分析；

5.通过[用户分群](data-analytics/user-segmentation.md)、[用户细查](data-analytics/individual-user-report.md)等来进行用户的分析。

6.通过配置[UTM广告参数](ads-tracking/utm-parameters.md#she-zhi-xiao-cheng-xu-luo-di-ye-de-utm)，来监测小程序投放的数据，计算投放的ROI。

![](.gitbook/assets/wei-xin-tou-tu-10.1722.png)

