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
* [6.API接口详细](ads-tracking-api.md#api-interface)
  * [6.1 应用 API](ads-tracking-api.md#app-api)
  * [6.2 推广活动相关 API](ads-tracking-api.md#markerting-api)
  * [6.3 渠道管理 API](ads-tracking-api.md#channel-api)
  * [6.4 链接创建 API](ads-tracking-api.md#link-api)
  * [6.5 Deeplink API](ads-tracking-api.md#deeplink-api)
  * [6.6 Onelink API](ads-tracking-api.md#6-6-onelink-api)
  * [6.7 Normallink API](ads-tracking-api.md#6-7-normallink-api)

### 1.概述 <a id="introduction"></a>

为满足广大客户更灵活创建广告监测链接的诉求，GrowingIO（以下简称GIO）提供了一套创建监测链接的API。本文档旨在说明一些调用流程，逻辑及相关接口说明。

### 2.名词及概念解释 <a id="terminology"></a>

GIO广告监测链接信息架构

![](../.gitbook/assets/image%20%28183%29.png)

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
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

### 6.API接口详细 <a id="api-interface"></a>

#### 6.1 应用 API <a id="app-api"></a>

新建应用请在GIO后台操作，此接口仅提供应用ID的查询。 

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/products` 



Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 产品编号 | gnPNkoWA |
| name | String | 名字 | GrowingIO 测试产品 |
| displayName | String | 产品显示名称，展示在deeplink页面 | gio |
| activated | Bool | 是否有效 | true |
| spn | String | spn | www.gioee.com |
| urlSchema | String | 产品的url schema | 8137d31f4e7b819f |
| platform | String | 平台 | ios |
| createdAt | Long | 创建时间 | 1480635903152 |

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

此部分相关接口可以查询已有活动的活动ID或者创建新的活动。 

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
| name | String | 名字 | 双十一推广 |

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

此相关部分API可以进行渠道的ID查询及新建渠道。 

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/channels` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 名字 | 二维码推广 |

示例：

```text
{
  "name":"线下地推"
}
```

GET `https://www.growingio.com/api/v1/projects/:project_id/meta/channels` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | channel id | gnPNkoWA |
| name | String | 名字 | 今日头条 |

Response 示例：

```text
[
  {
    "id": "gnPNkoWA",
    "name": "二维码推广"
  },
  {
    "id": "jinritoutiao",
    "name": "今日头条"
  }
]
```

#### 6.4 链接创建 API <a id="link-api"></a>

GIO目前提供三种类型的监测链接：Normal-Link，One-Link，Deep-Link。

 Normal-Link： 最常见的一种监测链接类型。适用于推广单个移动应用，引导用户进行APP的下载。场景包括广告平台投放等。 特别说明：广点通/智汇推/微信广告平台/Inmobi，此四个渠道由于创建过程中需要获取广告平台的相关参数，故不支持API创建以上平台的监测链接。

One-Link： 当需要在一条链接里推广两个移动应用（安卓版和iOS版）时，推荐使用 One-Link。适用于短信，微信，邮件等不明用户操作系统的推广场景。 

Deep-Link： 使用 Deep-Link 技术，已安装用户点击链接即可打开APP，未安装用户点击链接跳转到App Store。适用于通过短信/微信下的优惠券发送召唤用户重新打开并使用APP的场景。

请根据业务场景选择合适的监测链接。

#### **6.5 Deeplink API** <a id="deeplink-api"></a>

Deeplink链接创建逻辑：

![](../.gitbook/assets/growingio_tracking_api_3.png)

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/deeplinks`

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | 0523信息流推广 |
| `project_id` | String | 项目 UID | GQPDxPNm |
| productIdAndroid | String | Android产品ID \(从应用 API获取\) 选填,\(iOS Android 至少填一个\)  | Lj9yBRyD |
| productIdIos | String | iOS产品ID \(从应用 API获取\) 选填,\(iOS Android 至少填一个\) | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignIdIos | String | iOS 活动 id 选填 \(iOS Android 必填至少一个\) | gnPNkoWA |
| campaignIdAndroid | String | Android活动id 选填 \(iOS Android 必填至少一个\) | La9BwRne |
| downloadUrlIos | String | ios下载链接 选填 | [http://www.download.com](http://www.download.com/) |
| downloadUrlAndroid | String | Android下载链接 选填 | [http://www.download.com](http://www.download.com/) |
| iosParams | String | iOS 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| androidParams | String | Android 唤醒参数 | {"uri":"key1:value1&key2:value2"} |

示例：

```text
{
        "name": "ttsss223",
        "productIdIos": "GQPDxPNm",
        "channelId": "gnPNkoWA",
        "campaignIdIos": "GQPDxPNm",
        "downloadUrlIos": "http://www.growingio.com"
}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | GQPDxPNm |
| id | String | 资源id | GQPDxPNm |
| name | String | 链接名称 | 0523信息流推广 |
| productIdAndroid | String | Android 产品ID | Lj9yBRyD |
| productNameAndroid | String | 应用名称 | Growingio 测试产品 |
| productIdIos | String | iOS 产品ID |  |
| productNameIos | String | 应用名称 | Growingio 测试产品 |
| channelId | String | 渠道 id | gnPNkoWA |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignIdIos | String | iOS 活动 id | gnPNkoWA |
| campaignIdAndroid | String | Android 活动 id | La9BwRne |
| campaignNameIos | String | iOS 应用所属推广活动名称 | Growingio 测试 |
| campaignNameAndroid | String | Android 应用所属推广活动名称 | Growingio 测试 |
| downloadUrlIos | String | ios下载链接 | [http://www.download.com](http://www.download.com/) |
| downloadUrlAndroid | String | Android下载链接 | [http://www.download.com](http://www.download.com/) |
| iosParams | String | iOS 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| androidParams | String | Android 唤醒参数 | {"uri":"key1:value1&key2:value2"} |
| urlSchemaIos | String | ios url schema | "8137d31f4e7b819f" |
| urlSchemaAndroid | String | Android url scheme | "6137d41f4e7b819g" |
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
        "name": "ttss223",
        "projectId": "GQPDxPNm",
        "productIdIos": "GQPDxPNm",
        "productNameIos": "Growingio 测试产品",
        "productIdAndroid": null,
        "productNameAndroid": null,
        "trackingUrl": "https://gio.ren/d1ev8VE",
        "downloadUrlIos": "http://www.growingio.com",
        "downloadUrlAndroid": null,
        "urlSchemaIos": "8137d31f4e7b819f",
        "urlSchemaAndroid": null,
        "campaignIdIos": "GQPDxPNm",
        "campaignNameIos": "tt",
        "campaignIdAndroid": null,
        "campaignNameAndroid": null,
        "channelId": "GQPDxPNm",
        "channelName": "test",
        "status": "activated",
        "creatorId": "GQPDxPNm",
        "creatorName": "张溪梦",
        "updaterId": "GQPDxPNm",
        "updaterName": "张溪梦",
        "createdAt": 1525950922866,
        "updatedAt": 1525950922866
}
```

#### **6.6 Onelink API**

Onelink链接创建逻辑

 ![](../.gitbook/assets/growingio_tracking_api_4.png) 

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/onelinks`

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | 0523信息流推广 |
| `project_id` | String | 项目 UID | GQPDxPNm |
| productIdAndroid | String | Android产品ID, 必填 \(从应用 API获取\) | Lj9yBRyD |
| productIdIos | String | iOS产品ID, 必填 \(从应用 API获取\) | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignIdIos | String | iOS 活动 id 必填 | gnPNkoWA |
| campaignIdAndroid | String | Android活动id 必填 | La9BwRne |
| redirectUrl | String | 跳转链接 选填 | [http://www.download.com](http://www.download.com/) |

示例：

```text
{
    "name": "tt3ts",
    "productIdIos": "GQPDxPNm",
    "productIdAndroid": "GQPDxPNm",
    "redirectUrl": "http://www.download.com",
    "campaignIdIos": "GQPDxPNm",
    "campaignIdAndroid": "GQPDxPNm",
    "channelId": "GQPDxPNm"
}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | GQPDxPNm |
| id | String | 资源id | GQPDxPNm |
| name | String | 链接名称 | 0523信息流推广 |
| projectId | String |  项目 UID | GQPDxPNm |
| productIdAndroid | String | Android产品ID | GQPDxPNm |
| productNameAndroid | String | Android 产品名称 | Growingio 测试产品 |
| productIdIos | String | iOS产品ID | GQPDxPNm |
| productNameIos | String | iOS产品名称 | Growingio 测试产品 |
| trackingUrl | String | GrowingIO 分配的追踪链接 | [https://gio.ren/o2VQjBL](https://gio.ren/o2VQjBL) |
| redirectUrl | String | 目标链接 | [http://www.baidu.com](http://www.baidu.com/) |
| channelId | String | 渠道 id | gnPNkoWA |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignIdIos | String | iOS 活动ID | gnPNkoWA |
| campaignIdAndroid | String | Android 活动ID | La9BwRne |
| campaignNameIos | String | iOS 应用所属推广活动名称 | Growingio 测试 |
| campaignNameAndroid | String | Android 应用所属推广活动名称 | Growingio 测试 |
| status | String | 状态 | activated |
| creatorId | String | 创建人 id | GQPDxPNm |
| creatorName | String | 创建人名称 | 林生生 |
| updaterId | String | 最后更新人 id | GQPDxPNm |
| updaterName | String | 最后更新人名称 | 林生生 |
| createdAt | Long | 创建时间 | 1522411980015 |
| updatedAt | Long | 更新时间 | 1525768692277 |

示例：

```text
{
        "id": "Lj9yBRyD",
        "linkId": "o2VQjBL",
        "name": "tt3t",
        "projectId": "GQPDxPNm",
        "productIdIos": "GQPDxPNm",
        "productNameIos": "Growingio 测试产品",
        "productIdAndroid": "GQPDxPNm",
        "productNameAndroid": "Growingio 测试产品",
        "trackingUrl": "https://gio.ren/o2VQjBL",
        "redirectUrl": "http://www.baidu.com",
        "campaignIdIos": "GQPDxPNm",
        "campaignNameIos": "tt",
        "campaignIdAndroid": "GQPDxPNm",
        "campaignNameAndroid": "tt",
        "channelId": "GQPDxPNm",
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

#### **6.7 Normallink API**

Normallink链接创建逻辑：

 ![](https://docs.growingio.com/.gitbook/assets/ads_tracking_api_5.png)

POST `https://www.growingio.com/api/v1/projects/:project_id/meta/normallinks` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 链接名称,必填.长度50个字符内，同一个账号下系统会进行链接的同名校验，请勿重复提交同名链接。 | 0523信息流推广 |
| productId | String | 应用ID, 必填  | GQPDxPNm |
| channelId | String | 渠道 id，必填 | gnPNkoWA |
| campaignId | String | 活动 id 必填 | gnPNkoWA |
| redirectUrl | String | 跳转链接 选填 | [http://www.download.com](http://www.download.com/) |

示例：

```text
    {
        "name": "ttss23_Dsseeplink_iOS",
        "productId": "GQPDxPNm",
        "channelId": "GQPDxPNm",
        "campaignId": "GQPDxPNm",
        "redirectUrl": "http://www.growingio.com"
    }
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| linkId | String | 监测链接ID | GQPDxPNm |
| id | String | 资源id | GQPDxPNm |
| name | String | 链接名称 | 0523信息流推广 |
| projectId | String | 项目 UID | GQPDxPNm |
| productId | String | 产品ID | GQPDxPNm |
| appId | String | 产品的包名 | www.gioee.com\_ios |
| trackingUrl | String | GrowingIO 分配的追踪链接 | [https://gio.ren/o2VQjBL](https://gio.ren/o2VQjBL) |
| redirectUrl | String | 跳转链接  | [http://www.baidu.com](http://www.baidu.com/) |
| channelId | String | 渠道 id | gnPNkoWA |
| channelName | String | 渠道名称 | Growingio 测试渠道 |
| campaignId | String | 活动 id | gnPNkoWA |
| campaignName | String | 推广活动名称 | Growingio 测试 |
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
        "id": "QGPn2PYy",
        "linkId": "rPeJG3E",
        "name": "ttss23_Dseeplink_iOS",
        "projectId": "GQPDxPNm",
        "productId": "GQPDxPNm",
        "appId": "www.gioee.com_ios",
        "trackingUrl": "https://gio.ren/rPeJG3E",
        "redirectUrl": "http://www.growingio.com",
        "impressionUrl": null,
        "campaignId": "GQPDxPNm",
        "campaignName": "tt",
        "channelId": "GQPDxPNm",
        "channelName": "test",
        "status": "activated",
        "creatorId": "AwoVvo28",
        "creatorName": "系统创建",
        "updaterId": "AwoVvo28",
        "updaterName": "系统创建",
        "createdAt": 1527133238808,
        "updatedAt": 1527133238808
    }
```

