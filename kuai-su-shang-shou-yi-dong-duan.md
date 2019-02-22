---
description: 帮助您快速了解使用GrowingIO移动分析产品的上手步骤，快速了解相应功能。
---

# 快速上手-移动端

### GrowingIO 分析产品介绍

GrowingIO 是基于用户行为的新一代数据分析产品，吸取了国内外数据分析的最佳实践，创新了一整套数据采集、清洗整理、展现分析的一站式解决方案，帮助企业用数据驱动业务增长。我们希望：

* 帮助客户快速获取数据，实现快速洞察，促进业务快速迭代
* 帮助客户分析行为和业务数据，快速提高业务转化
* 帮助客户管理用户生命周期，精细化用户运营
* 沉淀数据资产，放心专享数据所有权

针对移动端产品数据分析，GrowingIO 提供了以下功能：

​**数据采集​**

1. 注册GrowingIO账号，进行应用管理。
2. 快速2分钟接入SDK，即可以通过无埋点采集，快速获得页面浏览、事件点击等行为数据。
3. 高级配置&自定义事件（埋点），进一步精细化满足自定义化数据采集需求。

​**预置分析内容-​概览​**

**2分钟接入SDK**，无需更多操作，**5分钟**即可获得实时**流量数据**；**1小时**即可**获得流量、页面访问等数据洞察。**

​**事件快速定义​**

通过“App圈选”快速定义页面、元素等关键用户行为，满足您产品运营**轻松获得数据洞察**、**快速迭代**的增长需求；通过“自定义事件和变量” 埋点定义数据功能，精细化获得各种业务属性数据。

​**产品分析、用户分析、广告监测等深度功能​**

使用广告监测、事件分析、漏斗分析、留存分析、用户分群和用户细查，提升投放、产品迭代、用户运营的转化效率。

### 开始使用

#### 第一步：集成SDK并进行配置  <a id="di-yi-bu-ji-cheng-sdk-bing-jin-hang-pei-zhi"></a>

1. 集成移动端 SDK； 开发者文档
   1. [iOS SDK](sdk-integration/ios-sdk-1/ios-sdk.md)
   2. [Android SDK](sdk-integration/android-sdk/android-sdk.md)
2. 配置完成进入[数据校验](sdk-integration/growingio-debugger/#growingio-mobile-debugger)，确认数据采集

#### 第二步：开始查看系统提供的数据内容 <a id="di-er-bu-kai-shi-cha-kan-xi-tong-ti-gong-de-shu-ju-nei-rong"></a>

系统提供了部分预置看板，包括“实时”，和“概览”。

* [实时](dashboard/realtime.md)； 每5分钟更新一次数据，帮助您即时了解流量情况。
* [概览](dashboard/overview.md)；整体描述你的移动端的访问、使用时长、留存、来源等数据情况。

#### 第三步：开始探索使用数据定义和分析工具：  <a id="di-san-bu-kai-shi-tan-suo-shi-yong-shu-ju-ding-yi-he-fen-xi-gong-ju"></a>

* [了解预置维度和指标的定义](data-model/olap-model/predifined-metrics-dimensions.md)；
* 了解“App圈选 （[文档](data-definition/circle/app/) \| [使用手册](http://growing.cn-bj.ufileos.com/app_circle.pdf)）”；尝试定义几个核心的产品事件，例如“加入购物车”、“浏览详情页”等。
* 探索几个基础但强大的分析工具
  * [事件分析](data-analytics/event-analysis.md)
  * [漏斗分析](data-analytics/funnel-analysis.md)
  * [留存分析](data-analytics/retention-analysis.md)
  * [App热图](data-analytics/heatmap/heatmap-app.md)
* 使用看板来组织你自己的内容
  * [看板](dashboard/)

#### 第四步：进行获客推广管理配置 <a id="di-si-bu-jin-hang-huo-ke-tui-guang-guan-li-pei-zhi"></a>

* 推广App，[使用GrowingIO移动端广告监测功能](ads-tracking/app-marketing.md)，包括普通监测跳转链接，以及deeplink链接 （Deep-Link [文档](https://docs.growingio.com/docs/ads-tracking/app-marketing#113)），精细化了解移动端推广，提升转化。
* 具体介绍也可以观看[视频](https://s.growingio.com/jvoRdB)。

### 进阶使用

* 使用分析手册，多来使用几个核心功能，并探索一些高级功能
  * 事件分析 [文档](data-analytics/event-analysis.md) \| [使用手册](https://s.growingio.com/nvN9MB)
  * 漏斗分析 [文档](data-analytics/funnel-analysis.md) \| [使用手册](https://s.growingio.com/9PXbR0) \| [视频](https://s.growingio.com/kKdDjv)
  * 留存分析 [文档](data-analytics/retention-analysis.md) \| [使用手册](https://s.growingio.com/p8QD3x) \| [视频](https://s.growingio.com/4PpoAK)
  * 智能路径 [文档](data-analytics/pathfinder.md)
  * 留存魔法师 [文档](data-analytics/magic-number.md)
* 探索用户分析几个功能
  * 用户分群 [文档](data-analytics/user-segmentation.md) \| [使用手册](https://s.growingio.com/9PaAZ8) \|  [视频](https://s.growingio.com/ambRb4)
  * [用户细查](data-analytics/individual-user-report.md)
  * [活跃用户分析](data-analytics/user-engagement-analysis.md)

### 其他需要进一步了解的功能    

* **分析人员（包括分析师、产品经理、运营等）**
  * [数据模型](data-model/)，帮助您进一步了解GrowingIO的数据采集口径，行为分析的数据逻辑。
  * 了解[埋点事件使用场景](data-model/event-model/custom-event/)，明确哪些场景需要进一步上报业务属性数据，以及怎么使用。
  * [用户变量](data-definition/user-variable/loginuserid.md)，进一步打通您的业务用户和访问用户。
  * 了解SDK 中支持部分重要场景（例如采集时间、banner等）的配置，结合需求，和工程师一起进行重要项目的配置。（[Android](sdk-integration/android-sdk/android-sdk.md#zhong-yao-pei-zhi) 或 [iOS](sdk-integration/ios-sdk-1/ios-sdk.md#zhong-yao-pei-zhi)）
* **开发者**
  * 了解**SDK的高级配置**\([Android](sdk-integration/android-sdk/android-sdk.md#zhong-yao-pei-zhi) 或 [iOS](sdk-integration/ios-sdk-1/ios-sdk.md#zhong-yao-pei-zhi)\)，根据数据校验情况进行高级配置
  * [设置上报自定义事件和变量（埋点）](data-definition/mina.md)
  * [API](api/)
* **系统管理员**
  * [系统项目高级配置](configuration/)，包括权限管理，用户管理，映射管理，应用管理等等。   

### 主要功能使用场景和手册

我们提供了一些主要功能的产品使用手册，希望可以为你带来帮助：

1. [指标和维度使用手册](https://s.growingio.com/NLdx0O)
2. [事件分析使用手册](https://s.growingio.com/nvN9MB)
3. [漏斗分析使用手册](https://s.growingio.com/9PXbR0)
4. [留存分析使用手册](https://s.growingio.com/p8QD3x)
5. [用户分群使用手册](https://s.growingio.com/9PaAZ8)

