# 广告监测链接创建服务 API

* [1.概述](ads-tracking-api.md#introduction)
* [2.名词及概念解释](ads-tracking-api.md#terminology)
* [3.系统校验规则说明](ads-tracking-api.md#explaination)
  * [3.1 AI 与应用相关](ads-tracking-api.md#ai-product)
  * [3.2 推广活动相关](ads-tracking-api.md#marketing-info)
  * [3.3 渠道相关](ads-tracking-api.md#channel-info)
  * [3.4 监测链接相关](ads-tracking-api.md#link-info)
* [4.使用流程](ads-tracking-api.md#user-flow)
* [5.认证说明](ads-tracking-api.md#authentication)
* [6.API 接口详细](ads-tracking-api.md#6api-jie-kou-xiang-xi)
  * [6.1 应用 API](ads-tracking-api.md#app-api)
  * [6.2 推广活动相关 API](ads-tracking-api.md#markerting-api)
  * [6.3 渠道管理 API](ads-tracking-api.md#channel-api)
  * [6.4 链接创建 API](ads-tracking-api.md#link-api)
    * [6.4.1 ](ads-tracking-api.md#deeplink-api)[吸引用户直接打开 App](ads-tracking-api.md#deeplink-api)
    * [6.4.2 同时推广 IOS 和 Android 两个平台](ads-tracking-api.md#onelink)
    * [6.4.3 推广 IOS 或 Android 单个平台](ads-tracking-api.md#normallink)

### 1.概述 <a id="introduction"></a>

为满足广大客户更灵活创建广告监测链接的诉求，GrowingIO（以下简称GIO）提供了一套创建监测链接的API。本文档旨在说明一些调用流程，逻辑及相关接口说明。

### 2.名词及概念解释 <a id="terminology"></a>

GIO广告监测链接信息架构

![](../.gitbook/assets/image%20%28184%29.png)

因此，GIO中生成一条监测链接至少需要涵盖以下信息： 

* 项目：项目ID ，即 AI 可在项目管理的项目概览里获得这串ID，也是集成 SDK 时 setAccountId 所用的部分。 
* 应用：应用ID，推广应用在GIO后台的分配的唯一应用ID。 
* 推广活动：自定义的一个维度。示例：百度信息流推广，湖南区域推广等。
* 监测链接：一条链接（或二维码），可跟踪后续的点击，激活等事件。

### 3.系统校验规则说明 <a id="explaination"></a>

#### 3.1 AI 与应用相关： <a id="ai-product"></a>

规则一：推广的应用是否隶属于当前AI。

#### 3.2 推广活动相关： <a id="marketing-info"></a>

规则一：同AI下推广活动不能重名。 规则二：推广活动名称限制50个字符，仅支持中英文数字-/\_,。

#### 3.3 渠道相关： <a id="channel-info"></a>

规则一：同AI下渠道名称不能重名，包括自定义渠道及系统预定义渠道。 规则二：渠道名称限制50个字符，仅支持中英文数字-/\_,。

#### 3.4 监测链接相关 <a id="link-info"></a>

规则一：同AI下监测链接不能重名。 规则二：监测链接名称限制50个字符，仅支持中英文数字-/\_,。 规则三：针对跳转地址有URL基本校验（是否可跳转，格式校验）。 规则四：必填校验。详见后续不同监测链接的创建逻辑。

### 4.使用流程 <a id="user-flow"></a>

为保证数据安全，GrowingIO所有的API服务，请求Head中需要携带Token。

Token获取详见：[“GrowingIO接口认证”文档](authentication.md)



完整的监测链接创建流程见下图： 

![](../.gitbook/assets/growingio_tracking_api_2.png)

### 5.认证说明 <a id="authentication"></a>

详细的认证过程请参考：[认证说明](authentication.md#authorization)

| 名字 | 类型 | 描述 | 示例 |
| :--- | :--- | :--- | :--- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | AoSoAAe9RZBmeb4R9zZd8YopJKcNIo5tozLZ9PYmDz2PbiHexQc0gAZ0R03qshRY |
| Authorization | String | 认证后获取到的 Token | e16b3ccd3e2a4e1e995db36435837828 |

### 6.API 接口详细

所有接口的路径参数在 POST 和 GET 的意义相同，示例数据仅供形式上的参考：

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| project\_id | String | 项目UID | LlFEWfn |

#### 6.1 应用 API <a id="app-api"></a>

新建应用请在GIO后台操作，此接口仅提供应用ID的查询。（GET 和 POST 请求Head中均需要携带Token）

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/products` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 产品编号 | GQPDxPNm |
| name | String | 名字 | Growingio 测试产品 |
| displayName | String | 产品显示名称，展示在deeplink页面 | gio |
| activated | Bool | 是否有效 | true |
| spn | String | spn | www.gioee.com |
| urlSchema | String | 产品的url schema | 8137d31f4e7b819f |
| platform | String | 平台 | ios |
| createdAt | Long | 创建时间 | 1522019721098 |

Response 示例：

```text
[
    {
        "displayName": "renrendai",
        "name": "renrendai",
        "activated": true,
        "spn": "com.hecom.Guanghua",
        "id": "Lj9yBRyD",
        "createdAt": 1480635903152,
        "urlSchema": "8137d31f4e7b819f",
        "platform": "android"
    },
    {
        "displayName": "gio",
        "name": "Growingio 测试产品",
        "activated": true,
        "spn": "www.gioee.com",
        "id": "GQPDxPNm",
        "createdAt": 1522019721098,
        "urlSchema": "8137d31f4e7b819f",
        "platform": "ios"
    }
]
```

#### 6.2 推广活动相关 API <a id="markerting-api"></a>

此部分相关接口可以查询已有活动的活动ID或者创建新的活动。（GET 和 POST 请求Head中均需要携带Token）

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/campaigns` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 名字 | 双十一推广 |

示例：

```text
{
  "name":"双十一推广"
}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 活动 id | gnPNkoWA |
| name | String | 名字 | 双十一推广 |

示例：

```text
{
  "id": "gnPNkoWA",
  "name":"双十一推广"
}
```

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/campaigns` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | campaign id | gnPNkoWA |
| name | String | 名字 | 大太阳活动 |

Response 示例：

```text
[
  {
    "id": "gnPNkoWA",
    "name": "大太阳活动"
  },
  {
    "id": "La9BwRne",
    "name": "美丽星辰"
  }
]
```

####  6.3 渠道管理 API <a id="channel-api"></a>

此相关部分API可以进行渠道的ID查询及新建渠道。（GET 和 POST 请求Head中均需要携带Token） 

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/channels` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 名字 | 二维码推广 |

示例：

```text
{
  "name":"二维码推广"
}
```

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/channels` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | channel id | HnKoPtrq |
| name | String | 名字 | 今日头条 |

Response 示例：

```text
[
  {
    "id": "gnPNkoWA",
    "name": "二维码推广"
  },
  {
    "id": "HnKoPtrq",
    "name": "今日头条"
  }
]
```

#### 6.4 链接创建 API <a id="link-api"></a>

GIO目前提供两种类型的监测链接：吸引用户直接打开 App，增加 App 下载量。

其中增加 App 下载量又分为：

1. 推广 IOS 或 Android 单个平台
2. 同时推广 IOS 和 Android 两个平台

 增加 App 下载量： 最常见的一种监测链接类型。第一种适用于推广单个移动应用，引导用户进行APP的下载。场景包括广告平台投放等。 特别说明：广点通/智汇推/微信广告平台/Inmobi，此四个渠道由于创建过程中需要获取广告平台的相关参数，故不支持API创建以上平台的监测链接。当需要在一条链接里推广两个移动应用（安卓版和iOS版）时，推荐使用第二种。适用于短信，微信，邮件等不明用户操作系统的推广场景。 

吸引用户直接打开 App： 使用 Deep-Link 技术，已安装用户点击链接即可打开APP，未安装用户点击链接跳转到App Store。适用于通过短信/微信下的优惠券发送召唤用户重新打开并使用APP的场景。

请根据业务场景选择合适的监测链接。

#### **6.4.1** 吸引用户直接打开 App <a id="deeplink-api"></a>

链接创建逻辑（GET 和 POST 请求Head中均需要携带Token）：

![](../.gitbook/assets/growingio_tracking_api_3.png)

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/deeplinks`

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | 0523信息流推广 |
| project\_id | String | 项目 UID | pRPDxPNm |
| productIdAndroid | String | Android产品ID \(从应用 API获取\) 选填,\(iOS Android 至少填一个\)  | Lj9yBRyD |
| productIdIos | String | iOS产品ID \(从应用 API获取\) 选填,\(iOS Android 至少填一个\) | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignIdIos | String | iOS 活动 id 选填 \(iOS Android 必填至少一个\) | GHxDxPNm |
| campaignIdAndroid | String | Android活动id 选填 \(iOS Android 必填至少一个\) | La9BwRne |
| downloadUrlIos | String | ios下载链接 选填 | http://www.growingio.com |
| downloadUrlAndroid | String | Android下载链接 选填 | http://www.growingio.com |
| iosParams | String | iOS 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| androidParams | String | Android 唤醒参数 | {"uri":"key1:value1&key2:value2"} |

示例：

```text
{
        "name": "0523信息流推广",
        "productIdIos": "GQPDxPNm",
        "channelId": "gnPNkoWA",
        "campaignIdIos": "GHxDxPNm",
        "downloadUrlIos": "http://www.growingio.com"
}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | d1ev8VE |
| id | String | 资源id | GQPDxPNm |
| name | String | 链接名称 | 0523信息流推广 |
| trackingUrl | String | 监测链接 | https://gio.ren/d1ev8VE |
| productIdAndroid | String | Android 产品ID | Lj9yBRyD |
| productNameAndroid | String | 应用名称 | Growingio Android测试产品 |
| productIdIos | String | iOS 产品ID | PNgtkoWQ |
| productNameIos | String | 应用名称 | Growingio IOS测试产品 |
| channelId | String | 渠道 id | KpPNkoWA |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignIdIos | String | iOS 活动 id | gnPNkoWA |
| campaignIdAndroid | String | Android 活动 id | La9BwRne |
| campaignNameIos | String | iOS 应用所属推广活动名称 | Growingio 测试 |
| campaignNameAndroid | String | Android 应用所属推广活动名称 | Growingio 测试 |
| downloadUrlIos | String | ios下载链接 | http://www.download.com |
| downloadUrlAndroid | String | Android下载链接 | http://www.download.com |
| iosParams | String | iOS 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| androidParams | String | Android 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| urlSchemaIos | String | ios url schema | 8137d31f4e7b819f |
| urlSchemaAndroid | String | Android url scheme | 6137d41f4e7b819g |
| status | String | 状态 | activated |
| creatorId | String | 创建人 id | GQPDxPNm |
| creatorName | String | 创建人名称 | 张溪梦 |
| updaterId | String | 最后更新人 id | GQPDxPNm |
| updaterName | String | 最后更新人名称 | 张溪梦 |
| createdAt | Long | 创建时间 | 1522411980015 |
| updatedAt | Long | 更新时间 | 1525768692277 |

示例：

```text
{
        "id": "nxog09md",
        "linkId": "d1ev8VE",
        "name": "0523信息流推广",
        "projectId": "pRPDxPNm",
        "productIdIos": "PNgtkoWQ",
        "productNameIos": "Growingio IOS测试产品",
        "productIdAndroid": "Lj9yBRyD",
        "productNameAndroid": "Growingio Android测试产品",
        "trackingUrl": "https://gio.ren/d1ev8VE",
        "downloadUrlIos": "http://www.download.com",
        "downloadUrlAndroid": "http://www.download.com",
        "urlSchemaIos": "8137d31f4e7b819f",
        "urlSchemaAndroid": 6137d41f4e7b819g,
        "campaignIdIos": "gnPNkoWA",
        "campaignNameIos": "Growingio 测试",
        "campaignIdAndroid": "La9BwRne",
        "campaignNameAndroid": Growingio 测试,
        "channelId": "KpPNkoWA",
        "channelName": "test",
        "status": "activated",
        "creatorId": "GQPDxPNm",
        "creatorName": "张溪梦",
        "updaterId": "GQPDxPNm",
        "updaterName": "张溪梦",
        "createdAt": 1522411980015,
        "updatedAt": 1525768692277
}
```

查询链接：

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/deeplinks`

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 资源id | GQPDxPNm |
| linkId | String | 监测链接id | dGVr8e9 |
| name | String | 链接名称 | 321deeplink |
| projectId | String | 项目uid | 4PYJMWoM |
| productIdIos | String | ios产品id | xRxVp0o5 |
| productNameIos | String | ios产品名称 | TestAPP |
| productIdAndroid | String | android产品id | LPdgKARN |
| productNameAndroid | String | android产品名称 | Android SDK Demo |
| trackingUrl | String | 监测链接 | https://gio.ren/dGVr8e9 |
| downloadUrlIos | String | ios下载链接 | http://baidu.com |
| downloadUrlAndroid | String | android下载链接 | http://growingio.com |
| urlSchemaIos | String | ios url schema | c35abef955cd913a |
| urlSchemaAndroid | String | android url schema | 8137d31f4e7b819f |
| campaignIdIos | String | ios活动id | j9yB1nRy |
| campaignNameIos | String | ios活动名称 | 321上线 |
| campaignIdAndroid | String | android活动id | xoga4DRm |
| campaignNameAndroid | String | android活动名称 | 321上线验证1 |
| iosParams | String | ios唤醒参数 | null |
| androidParams | String | android唤醒参数 | null |
| channelId | String | 渠道id | 34RzeX9V |
| channelName | String | 渠道名称 | 123测试编辑编辑 |
| status | String | 状态 | activated |
| creatorId | String | 创建人id | nRbm8d93 |
| creatorName | String | 创建人名称 | xx |
| updaterId | String | 最后更新人id | nRbm8d93 |
| updaterName | String | 最后更新人名称 | xx |
| createdAt | Long | 创建时间 | 1521642287367 |
| updatedAt | Long | 更新时间 | 1521642287367 |

示例：

```text
[
    {
        "id": "GQPDxPNm",
        "linkId": "dGVr8e9",
        "name": "321deeplink",
        "projectId": "4PYJMWoM",
        "productIdIos": "xRxVp0o5",
        "productNameIos": "TestAPP",
        "productIdAndroid": "LPdgKARN",
        "productNameAndroid": "Android SDK Demo",
        "trackingUrl": "https://gio.ren/dGVr8e9",
        "downloadUrlIos": "http://baidu.com",
        "downloadUrlAndroid": "http://growingio.com",
        "urlSchemaIos": "c35abef955cd913a",
        "urlSchemaAndroid": "8137d31f4e7b819f",
        "campaignIdIos": "j9yB1nRy",
        "campaignNameIos": "321上线",
        "campaignIdAndroid": "xoga4DRm",
        "campaignNameAndroid": "321上线验证1",
        "iosParams": null,
        "androidParams": null,
        "channelId": "34RzeX9V",
        "channelName": "123测试编辑编辑",
        "status": "activated",
        "creatorId": "nRbm8d93",
        "creatorName": "xx",
        "updaterId": "nRbm8d93",
        "updaterName": "xx",
        "createdAt": 1521642287367,
        "updatedAt": 1521642287367
    },
    {
        "id": "DnRbVR3g",
        "linkId": "dG3pzBA",
        "name": "上线验证",
        "projectId": "4PYJMWoM",
        "productIdIos": "rREJ88PL",
        "productNameIos": "RnTestiOS",
        "productIdAndroid": "j9yEwm9y",
        "productNameAndroid": "renrenda",
        "trackingUrl": "https://gio.ren/dG3pzBA",
        "downloadUrlIos": "http://baidu.com",
        "downloadUrlAndroid": "http://growingio.com",
        "urlSchemaIos": "80310c35a53c9a45",
        "urlSchemaAndroid": "8137d31f4e7b819f",
        "campaignIdIos": "6LPdk4RN",
        "campaignNameIos": "111_ios",
        "campaignIdAndroid": "6WoMxNPk",
        "campaignNameAndroid": "EEE",
        "iosParams": null,
        "androidParams": null,
        "channelId": "gnPNgloW",
        "channelName": "测试同名渠道新建删除",
        "status": "activated",
        "creatorId": "nRbm8d93",
        "creatorName": "xx",
        "updaterId": "nRbm8d93",
        "updaterName": "xx",
        "createdAt": 1523969909463,
        "updatedAt": 1523969909463
    }
]
```

#### **6.4.2** 同时推广 IOS 和 Android 两个平台 <a id="onelink"></a>

链接创建逻辑（GET 和 POST 请求Head中均需要携带Token）：

 ![](../.gitbook/assets/growingio_tracking_api_4.png) 

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/onelinks`

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | tt3ts |
| project\_id | String | 项目 UID | GQPDxPNm |
| productIdAndroid | String | Android产品ID, 必填 \(从应用 API获取\) | Lj9yBRyD |
| productIdIos | String | iOS产品ID, 必填 \(从应用 API获取\) | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignIdIos | String | iOS 活动 id 必填 | ONPNkorA |
| campaignIdAndroid | String | Android活动id 必填 | La9BwRne |
| redirectUrl | String | 跳转链接 选填 | http://www.download.com |

示例：

```text
{
    "name": "tt3ts",
    "productIdIos": "GQPDxPNm",
    "productIdAndroid": "Lj9yBRyD",
    "redirectUrl": "http://www.download.com",
    "campaignIdIos": "ONPNkorA",
    "campaignIdAndroid": "La9BwRne",
    "channelId": "gnPNkoWA"
}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | o2VQjBL |
| id | String | 资源id | Lj9yBRyD |
| name | String | 链接名称 | tt3ts |
| projectId | String |  项目 UID | GQPDxPNm |
| productIdAndroid | String | Android产品ID | nTeDxPNm |
| productNameAndroid | String | Android 产品名称 | Growingio IOS测试产品 |
| productIdIos | String | iOS产品ID | GQPDxytE |
| productNameIos | String | iOS产品名称 | Growingio Android测试产品 |
| trackingUrl | String | GrowingIO 分配的追踪链接 | https://gio.ren/o2VQjBL |
| redirectUrl | String | 目标链接 | http://www.growingio.com |
| channelId | String | 渠道 id | OpPDxYtW |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignIdIos | String | iOS 活动ID | PQyBRPTq |
| campaignIdAndroid | String | Android 活动ID | GQyBRPNm |
| campaignNameIos | String | iOS 应用所属推广活动名称 | tt |
| campaignNameAndroid | String | Android 应用所属推广活动名称 | tt |
| status | String | 状态 | activated |
| creatorId | String | 创建人 id | MkJDxPNm |
| creatorName | String | 创建人名称 | 张溪梦 |
| updaterId | String | 最后更新人 id | MkJDxPNm |
| updaterName | String | 最后更新人名称 | 张溪梦 |
| createdAt | Long | 创建时间 | 1522411980015 |
| updatedAt | Long | 更新时间 | 1525768692277 |

示例：

```text
{
        "id": "Lj9yBRyD",
        "linkId": "o2VQjBL",
        "name": "tt3t",
        "projectId": "GQPDxPNm",
        "productIdIos": "GQPDxytE",
        "productNameIos": "Growingio IOS测试产品",
        "productIdAndroid": "nTeDxPNm",
        "productNameAndroid": "Growingio Android测试产品",
        "trackingUrl": "https://gio.ren/o2VQjBL",
        "redirectUrl": "http://www.growingio.com",
        "campaignIdIos": "PQyBRPTq",
        "campaignNameIos": "tt",
        "campaignIdAndroid": "GQyBRPNm",
        "campaignNameAndroid": "tt",
        "channelId": "OpPDxYtW",
        "channelName": "Growingio 测试渠道",
        "status": "activated",
        "creatorId": "MkJDxPNm",
        "creatorName": "张溪梦",
        "updaterId": "MkJDxPNm",
        "updaterName": "张溪梦",
        "createdAt": 1522411980015,
        "updatedAt": 1525768692277
}
```

查询链接：

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/onelinks`

Response: Status Code: 200 OK

示例：

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 资源id | nxog09md |
| linkId | String | 监测链接id | dGVr8e9 |
| name | String | 链接名称 | qwe |
| projectId | String | 项目uid | 4PYJMWoM |
| productIdIos | String | ios产品id | xRxVp0o5 |
| productNameIos | String | ios产品名称 | TestAPP |
| productIdAndroid | String | android产品id | j9yEwm9y |
| productNameAndroid | String | android产品名称 | 123测试编辑编辑 |
| trackingUrl | String | 监测链接 | https://gio.ren/dGVr8e9 |
| redirectUrl | String | 跳转链接 | http://123.com |
| campaignIdIos | String | ios活动id | j9yB1nRy |
| campaignNameIos | String | ios活动名称 | 321上线 |
| campaignIdAndroid | String | android活动id | xoga4DRm |
| campaignNameAndroid | String | android活动名称 | 321上线验证1 |
| channelId | String | 渠道id | 34RzeX9V |
| channelName | String | 渠道名称 | 123测试编辑编辑 |
| status | String | 状态 | activated |
| creatorId | String | 创建人id | nRbm8d93 |
| creatorName | String | 创建人名称 | xx |
| updaterId | String | 最后更新人id | nRbm8d93 |
| updaterName | String | 最后更新人名称 | xx |
| createdAt | Long | 创建时间 | 1521642287367 |
| updatedAt | Long | 更新时间 | 1521642287367 |

示例：

```text
[
    {
        "id": "nxog09md",
        "linkId": "dGVr8e9",
        "name": "qwe",
        "projectId": "4PYJMWoM",
        "productIdIos": "xRxVp0o5",
        "productNameIos": "TestAPP",
        "productIdAndroid": "j9yEwm9y",
        "productNameAndroid": "123测试编辑编辑",
        "trackingUrl": "https://gio.ren/dGVr8e9",
        "redirectUrl": "http://123.com",
        "campaignIdIos": "O4PKxoEb",
        "campaignNameIos": "321上线",
        "campaignIdAndroid": "xoga4DRm",
        "campaignNameAndroid": "321上线验证1",
        "channelId": "34RzeX9V",
        "channelName": "123测试编辑编辑",
        "status": "activated",
        "creatorId": "nRbm8d93",
        "creatorName": "xx,
        "updaterId": "nRbm8d93",
        "updaterName": "xx",
        "createdAt": 1521642287367,
        "updatedAt": 1521642287367
    },
    {
        "id": "KnoqDPka",
        "linkId": "KnoqDPka_o",
        "name": "测试123",
        "projectId": "4PYJMWoM",
        "productIdIos": "xRxVp0o5",
        "productNameIos": "TestAPP",
        "productIdAndroid": "LPdgKARN",
        "productNameAndroid": "Android SDK Demo",
        "trackingUrl": "https://t.growingio.com/app/at2/KnoqDPka_o",
        "redirectUrl": "http://baidu.com",
        "campaignIdIos": "vkorEP14",
        "campaignNameIos": "上线验证_ios",
        "campaignIdAndroid": "5z98l9ax",
        "campaignNameAndroid": "上线验证_android",
        "channelId": "K5RpNzoN",
        "channelName": "onelink上线",
        "status": "activated",
        "creatorId": "nRbm8d93",
        "creatorName": "xx",
        "updaterId": "nRbm8d93",
        "updaterName": "xx",
        "createdAt": 1509631329798,
        "updatedAt": 1509631329798
    }
]
```

#### **6.4.3** 推广 IOS 或 Android 单个平台 <a id="normallink"></a>

链接创建逻辑（GET 和 POST 请求Head中均需要携带Token）：

 ![](https://docs.growingio.com/.gitbook/assets/ads_tracking_api_5.png)

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/normallinks` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | 0523信息流推广 |
| productId | String | 应用ID, 必填  | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignId | String | 活动 id 必填 | UPLNkoWA |
| redirectUrl | String | 跳转链接 选填 | http://www.growingio.com |

示例：

```text
    {
        "name": "0523信息流推广",
        "productId": "GQPDxPNm",
        "channelId": "gnPNkoWA",
        "campaignId": "UPLNkoWA",
        "redirectUrl": "http://www.growingio.com"
    }
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | rPeJG3E |
| id | String | 资源id | QGPn2PYy |
| name | String | 链接名称 | ttss23\_Dseeplink\_iOS |
| projectId | String | 项目 UID | GQPDxPNm |
| productId | String | 产品ID | PTPDxPNm |
| appId | String | 产品的包名 | www.gioee.com\_ios |
| trackingUrl | String | GrowingIO 分配的追踪链接 | https://gio.ren/rPeJG3E |
| redirectUrl | String | 跳转链接  | http://www.growingio.com |
| channelId | String | 渠道 id | GUVDxPNm |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignId | String | 活动 id | YtBDxPNm |
| campaignName | String | 推广活动名称 | Growingio 测试活动 |
| status | String | 状态 | activated |
| creatorId | String | 创建人 id | AwoVvo28 |
| creatorName | String | 创建人名称 | 系统创建 |
| updaterId | String | 最后更新人 id | AwoVvo28 |
| updaterName | String | 最后更新人名称 | 系统创建 |
| createdAt | Long | 创建时间 | 1527133238808 |
| updatedAt | Long | 更新时间 | 1527133238808 |

示例：

```text
{
        "id": "QGPn2PYy",
        "linkId": "rPeJG3E",
        "name": "ttss23_Dseeplink_iOS",
        "projectId": "GQPDxPNm",
        "productId": "PTPDxPNm",
        "appId": "www.gioee.com_ios",
        "trackingUrl": "https://gio.ren/rPeJG3E",
        "redirectUrl": "http://www.growingio.com",
        "impressionUrl": null,
        "campaignId": "YtBDxPNm",
        "campaignName": "Growingio 测试渠道",
        "channelId": "GUVDxPNm",
        "channelName": "Growingio 测试活动",
        "status": "activated",
        "creatorId": "AwoVvo28",
        "creatorName": "系统创建",
        "updaterId": "AwoVvo28",
        "updaterName": "系统创建",
        "createdAt": 1527133238808,
        "updatedAt": 1527133238808
    }
```

查询链接：

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/normallinks`

Response: Status Code: 200 OK

示例：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x540D;</th>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x683C;&#x5F0F;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x8D44;&#x6E90;id</td>
      <td style="text-align:left">La9BwRne</td>
    </tr>
    <tr>
      <td style="text-align:left">linkId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x76D1;&#x6D4B;&#x94FE;&#x63A5;id</td>
      <td style="text-align:left">LU5BwRnO</td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x94FE;&#x63A5;&#x540D;&#x79F0;</td>
      <td style="text-align:left">&#x6D4B;&#x8BD5;&#x94FE;&#x63A5;&#x4E8C;</td>
    </tr>
    <tr>
      <td style="text-align:left">projectId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;uid</td>
      <td style="text-align:left">4PYJMWoM</td>
    </tr>
    <tr>
      <td style="text-align:left">productId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x4EA7;&#x54C1;id</td>
      <td style="text-align:left">LPdgKARN</td>
    </tr>
    <tr>
      <td style="text-align:left">appId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">app&#x5305;&#x540D;</td>
      <td style="text-align:left">com.demo.app.androidsdkdemo_android</td>
    </tr>
    <tr>
      <td style="text-align:left">trackingUrl</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x76D1;&#x6D4B;&#x94FE;&#x63A5;</td>
      <td style="text-align:left">
        <p>https://t.growingio.com/app/</p>
        <p>at1/La9BwRne?idfa=</p>
        <p>__IDFA__&amp;idfa_md5</p>
        <p>=__MD5_IDFA__&amp;stm</p>
        <p>=__CLICK_TMS__</p>
        <p>&amp;ip=__CLIENT_IP</p>
        <p>__&amp;ua=__UA__&amp;callback_param</p>
        <p>=__APPKEY__</p>
        <p>&amp;source=__SOURCE_</p>
        <p>_&amp;sid=__SEARCH_ID_</p>
        <p>_&amp;clkid=__CLK_ID__</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">redirectUrl</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x8DF3;&#x8F6C;&#x94FE;&#x63A5;</td>
      <td style="text-align:left">null</td>
    </tr>
    <tr>
      <td style="text-align:left">impressionUrl</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x5E7F;&#x544A;&#x5C55;&#x793A;&#x94FE;&#x63A5;</td>
      <td style="text-align:left">null</td>
    </tr>
    <tr>
      <td style="text-align:left">campaignId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">ios&#x6D3B;&#x52A8;id</td>
      <td style="text-align:left">d4PYjoME</td>
    </tr>
    <tr>
      <td style="text-align:left">campaignName</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">ios&#x6D3B;&#x52A8;&#x540D;&#x79F0;</td>
      <td style="text-align:left">&#x5927;&#x591C;&#x5BB5;&#x6D3B;&#x52A8;</td>
    </tr>
    <tr>
      <td style="text-align:left">channelId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x6E20;&#x9053;id</td>
      <td style="text-align:left">GQPDxPNm</td>
    </tr>
    <tr>
      <td style="text-align:left">channelName</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x6E20;&#x9053;&#x540D;&#x79F0;</td>
      <td style="text-align:left">&#x591A;&#x76DF;</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x72B6;&#x6001;</td>
      <td style="text-align:left">activated</td>
    </tr>
    <tr>
      <td style="text-align:left">creatorId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x521B;&#x5EFA;&#x4EBA;id</td>
      <td style="text-align:left">nPNgQkoW</td>
    </tr>
    <tr>
      <td style="text-align:left">creatorName</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x521B;&#x5EFA;&#x4EBA;&#x540D;&#x79F0;</td>
      <td style="text-align:left">xx</td>
    </tr>
    <tr>
      <td style="text-align:left">updaterId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x6700;&#x540E;&#x66F4;&#x65B0;&#x4EBA;id</td>
      <td style="text-align:left">nPNgQkoW</td>
    </tr>
    <tr>
      <td style="text-align:left">updaterName</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x6700;&#x540E;&#x66F4;&#x65B0;&#x4EBA;&#x540D;&#x79F0;</td>
      <td style="text-align:left">xx</td>
    </tr>
    <tr>
      <td style="text-align:left">createdAt</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">&#x521B;&#x5EFA;&#x65F6;&#x95F4;</td>
      <td style="text-align:left">1521642287367</td>
    </tr>
    <tr>
      <td style="text-align:left">updatedAt</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">&#x66F4;&#x65B0;&#x65F6;&#x95F4;</td>
      <td style="text-align:left">1521642287367</td>
    </tr>
    <tr>
      <td style="text-align:left">params</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x817E;&#x8BAF;&#x793E;&#x4EA4;&#x5E7F;&#x544A;&#x53C2;&#x6570;</td>
      <td
      style="text-align:left">null</td>
    </tr>
  </tbody>
</table>示例：

```text
[
    {
        "id": "La9BwRne",
        "linkId": "LU5BwRnO",
        "name": "测试链接二",
        "projectId": "4PYJMWoM",
        "productId": "LPdgKARN",
        "appId": "com.demo.app.androidsdkdemo_android",
        "trackingUrl": "https://t.growingio.com/app/at1/La9BwRne?idfa=__IDFA__&idfa_md5=__MD5_IDFA__&stm=__CLICK_TMS__&ip=__CLIENT_IP__&ua=__UA__&callback_param=__APPKEY__&source=__SOURCE__&sid=__SEARCH_ID__&clkid=__CLK_ID__",
        "redirectUrl": null,
        "impressionUrl": null,
        "campaignId": "d4PYjoME",
        "campaignName": "大夜宵活动",
        "channelId": "GQPDxPNm",
        "channelName": "多盟",
        "status": "activated",
        "creatorId": "nPNgQkoW",
        "creatorName": "fowindhe111",
        "updaterId": "nPNgQkoW",
        "updaterName": "fowindhe111",
        "createdAt": 1521642287367,
        "updatedAt": 1521642287367,
        "params": null
    },
    {
        "id": "ebR7WRGz",
        "linkId": "ebR7WRGz",
        "name": "验证流程",
        "projectId": "4PYJMWoM",
        "productId": "LPdgKARN",
        "appId": "com.demo.app.androidsdkdemo_android",
        "trackingUrl": "https://t.growingio.com/app/at1/ebR7WRGz?imei=__IMEI__&imei_md5=__IMEIMD5__&stm=__CLICK_TMS__&ip=__CLIENT_IP__&ua=__UA__&callback_param=__APPKEY__&source=__SOURCE__&sid=__SEARCH_ID__&clkid=__CLK_ID__",
        "redirectUrl": null,
        "impressionUrl": null,
        "campaignId": "vnomv9zJ",
        "campaignName": "验证流程活动",
        "channelId": "GQPDxPNm",
        "channelName": "多盟",
        "status": "activated",
        "creatorId": "nPNgQkoW",
        "creatorName": "fowindhe111",
        "updaterId": "nPNgQkoW",
        "updaterName": "fowindhe111",
        "createdAt": 1499952124854,
        "updatedAt": 1499952124854,
        "params": null
    }
]
```

