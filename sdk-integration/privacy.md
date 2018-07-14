# 安全性说明

* [1.SDK工作方式](privacy.md#1)
  * [1.1 JS SDK](privacy.md#1-1)
  * [1.2 移动端 SDK](privacy.md#1-2)
* [2.数据安全说明](privacy.md#2)
* [3.员工访问数据说明](privacy.md#3)
* [4.符合欧盟《一般数据保护条例》 \(GDPR\)](privacy.md#4)
  * [4.1 接口内容](privacy.md#4-1)
  * [4.2 Andorid 样例](privacy.md#4-2)
  * [4.3 iOS 样例](privacy.md#4-3)
  * [4.4 JS 样例](privacy.md#4-4)

GrowingIO除了先进的数据分析理念和技术外，还从硅谷带来了对于数据安全与数据隐私极为重视的态度，我们将在每一个环节保证您的数据安全以及保护用户数据隐私。 在以下的文档中，我们将会为您介绍GrowingIO的数据采集的内容，数据安全的措施以及针对数据采集的控制项内容。

### 1.SDK工作方式 {#1}

#### 1.1 JS SDK {#1-1}

GrowingIO JS SDK 是运行于网页的一段 Javascript 代码，基于无埋点技术采集网站数据，同时 GrowingIO JS SDK 也提供丰富的接口以支持埋点。采集到的数据将被传输并存储在 GrowingIO 的云端服务器上。GrowingIO 通过使用这些数据来分析客户网站的用户的使用情况，生成网站使用报告，提供跟用户行为数据分析相关的服务。

GrowingIO JS SDK 会在网站用户加载网页后自动启动，并收集用户的行为数据，建议将 GrowingIO 提供的跟踪代码放在`<head> </head>`之间。JS SDK 采用异步方式加载，不会影响网站自身的加载数据。

目前 SDK 主要采集三类数据:

* 访问数据:网站访客在何时何地访问了哪个网页，收集信息包括域名、页面路径、浏览器、操作系统、屏幕分辨率、访问来源、用户唯一标识 ID、访问唯一标识 ID、访问时间、页面标题等。如果客户集成时设置了自定义维度，也会一并收集。
* 行为数据:用户在网站上的交互行为，比如点击链接、提交表单、修改选择，都会被自动采集。采集内容包括交互行为类型、交互元素的页面信息、交互元素的标记 ID、交互元素的超链接、交互元素的位置信息等。**GrowingIO 不采集任何用户在文本框中输入的信息，唯一例外是搜索框。**
* 元素浏览数据:当用户访问网站时，用户浏览的内容即页面出现的元素，会被自动采集，包括内容所在的页面信息、元素的标记 ID、文本内容、超链接、位置信息。

#### 1.2 移动端 SDK {#1-2}

移动端SDK需要在应用打包时，被加载在您的应用当中。GrowingIO的「移动端SDK」会随着客户应用的启动而自动开始进行用户行为数据。当用户关闭应用时，SDK会随着客户应用的关闭而关闭，不会在后台做任何额外动作。

**1.2.1 时间延迟**

经过我们反复的测量，移动端SDK的数据发送仅仅会带来10ms以内的时间延迟，用户感知不到任何的差异。GrowingIO真正的做到了用户无感知的数据采集，不会对应用的用户体验带来任何降低。

**1.2.2 稳定性**

我们非常注重SDK的稳定性，每个版本的SDK我们都会进行大量的稳定性测试，以确保您的应用一如既往的稳定。从目前客户集成SDK的结果来看，应用的崩溃率没有因为集成而提高。

**1.2.3 移动端SDK采集的数据类型**

与「JS SDK」一样，移动端SDK主要采集三类数据：访问数据，内容数据，行为数据。并且，不采集应用文本框里的数据，也就不会主动记录普通用户填写的账户/电话/银行卡等隐私信息，在采集环节保证安全。

### 2.数据安全说明 {#2}

GrowingIO使用SSL对数据传输进行加密。

在基于网络的计算环境中，数据和应用的安全性至关重要。使用 AWS 公有云，对数据进行隔离处理，对数据的收集、处理以及存储机制进行安全审查，保障客户的数据安全。

### 3.员工访问数据说明 {#3}

* 客户所有帐号数据都是机密，并受合同条款的约束。
* 未经客户明确许可，客户服务代表和技术支持人员不得访问客户级数据。
* 访问客户数据时，员工的活动范围仅限于履行工作职责所需的数据。
* 员工不得使用非 GrowingIO 所有或未经 GrowingIO 批准的网络设备访问数据。
* GrowingIO员工需要履行严格的合同保密义务，如果其未能履行这些义务，就可能会被追究法律责任或被终止其与GrowingIO的合同关系。

### 4.符合欧盟《一般数据保护条例》 \(GDPR\) {#4}

GrowingIO在Mobile SDK 2.3.2，JS SDK 2.1.8及以上版本中提供了以下的API供开发者调用满足客户网站或移动应用符合欧盟区的《一般数据保护条例》\(GDPR\)。

* GrowingIO SDK提供默认是否开启数据采集的配置项
* GrowingIO SDK提供关闭或开启全局数据采集的接口，开发者可在APP中任何场景时调用该接口
* GrowingIO SDK提供获取该设备的设备ID接口，开发者可配合数据侧提供的接口删除或导出该设备的行为数据

#### 4.1 接口内容 {#4-1}

| 接口名称 | Android | iOS | JS |
| --- | --- | --- | --- |
| 全局配置项 | .disableDataCollect\(\) |  |  |
| 关闭或开启全局数据采集 | // 不采集数据 GrowingIO.getInstance\(\).disableDataCollect\(\); // 收集数据 GrowingIO.getInstance\(\).enableDataCollect\(\) | disableDataCollect    enableDataCollect | // 开启gdpr，停止数据采集 window.gio\('config',{"dataCollect":"false"}\); // 关闭gdpr，开始数据采集 window.gio\('config',{"dataCollect":"true"}\); 放在send之前 |
| 获取访问用户ID | GrowingIO.getInstance\(\).getVisitUserId\(\); | getVisitUserId | window.gio\('getVisitUserId'\); 放在send之后 |

#### **4.2 Andorid 样例** {#4-2}

```text
GrowingIO.startWithConfiguration(this, new Configuration()
        .disableDataCollect()        // 开启GDPR， 不采集数据。 默认采集
        .useID()
        .trackAllFragments());
//  不采集数据
GrowingIO.getInstance().disableDataCollect();
// 收集数据
GrowingIO.getInstance().enableDataCollect();
// 获取设备Id
GrowingIO.getInstance().getVisitUserId();
```

#### **4.3 iOS 样例** {#4-3}

```text
// 开启GDPR
[Growing disableDataCollect];

// 关闭GDPR
[Growing enableDataCollect];

// 获取设备ID
NSString *viId = [Growing getVisitUserId];
```

#### **4.4 JS 样例** {#4-4}

```text
// 开启gdpr，停止数据采集

window.gio('config',{"dataCollect":"false"});

// 关闭gdpr，开始数据采集
window.gio('config',{"dataCollect":"true"});

获取访问用户ID
window.gio('getVisitUserId')   放在send之后
```

