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

注意：

* productId 需要在[应用 API](wxminp-qrcode-api.md#app-api)中查询获取。
* 在进行导出之前，请务必参考[“GrowingIO接口认证”文档](authentication.md)，完成接口认证获取 token 。



### 1.系统校验规则说明 <a id="explaination"></a>

#### 1.1 项目 ID \(AI\)  与应用相关： <a id="ai-product"></a>

规则一：推广的应用是否隶属于当前项目。

#### 1.2 推广活动相关： <a id="marketing-info"></a>

规则一：同项目下推广活动不能重名。 规则二：推广活动名称限制50个字符，仅支持中英文数字-/\_,。

#### 1.3 渠道相关： <a id="channel-info"></a>

规则一：同项目下渠道名称不能重名，包括自定义渠道及系统预定义渠道。 规则二：渠道名称限制50个字符，仅支持中英文数字-/\_,。

#### 1.4 监测链接相关 <a id="link-info"></a>

规则一：同项目下监测链接不能重名。 规则二：监测链接名称限制50个字符，仅支持中英文数字-/\_,。 规则三：针对跳转地址有URL基本校验（是否可跳转，格式校验）。 规则四：必填校验。详见后续不同监测链接的创建逻辑。

### 2.使用流程 <a id="user-flow"></a>

为保证数据安全，GrowingIO所有的API服务，请求Head中需要携带Token。

Token获取详见：[“GrowingIO接口认证”文档](authentication.md)

完整的监测链接创建流程见下图： 

![](../.gitbook/assets/growingio_tracking_api_2.png)

### 3.认证说明 <a id="authentication"></a>

详细的认证过程请参考：[认证说明](authentication.md#authorization)

| 名字 | 类型 | 描述 | 示例 |
| :--- | :--- | :--- | :--- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

### 4.API接口详细 <a id="api-interface"></a>

#### 4.1 应用 API <a id="app-api"></a>

新建应用请在GIO后台操作，此接口仅提供应用ID的查询。 

GET `https://www.growingio.com/api/v1/projects/{项目编号}/meta/products` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 产品编号 | gnPNkoWA |
| name | String | 名字 | GIO测试 SDK |
| displayName | String | 对应的 app id | LPdgKARN |
| activated | Bool | 是否有效 | true |
| spn | String | spn | wx51cba5e78d4ef4d8 |
| urlSchema | String | 产品的url schema | 8137d31f4e7b819f |
| platform | String | 平台 | minp |
| createdAt | Long | 创建时间 | 1480635903152 |

Response 示例：

```text
[    {        "displayName": "mina growth",        "name": "GIO测试 SDK",        "activated": true,        "spn": "wx51cba5e78d4ef4d8",        "id": "Lj9yBRyD",        "createdAt": 1480635903152,        "urlSchema": "8137d31f4e7b819f",        "platform": "minp"    },    {        "displayName": "gio",        "name": "Growingio 测试产品",        "activated": true,        "spn": "2018071560686136",        "id": "GQPDxPNm",        "createdAt": 1522019721098,        "urlSchema": "8137d31f4e7b819f",        "platform": "alip"    }]
```

#### 4.2 推广活动相关 API <a id="markerting-api"></a>

此部分相关接口可以查询已有活动的活动ID或者创建新的活动。 

POST `https://www.growingio.com/api/v1/projects/{项目编号}/meta/campaigns` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 名字 | 双十一推广 |
| productId | String | 对应 app 的 id | LPdgKARN |

示例：

```text
{  "productId":"rREJ88PL",  "name":"双十一推广"}
```

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | 活动 id | gnPNkoWA |
| productId | String | 对应 app 的 id | LPdgKARN |
| name | String | 名字 | 双十一推广 |

示例：

```text
{  "id": "gnPNkoWA",  "productId":"rREJ88PL",  "name":"双十一推广"}
```

GET `https://www.growingio.com/api/v1/projects/{项目编号}/meta/campaigns` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | campaign id | gnPNkoWA |
| name | String | 名字 | 双十一推广 |
| productId | String | 对应 app 的 id | LPdgKARN |

Response 示例：

```text
[  {    "id": "gnPNkoWA",    "name": "大太阳活动",    "productId": "LPdgKARN"  },  {    "id": "La9BwRne",    "name": "美丽星辰",    "productId": "LPdgKARN"  }]
```

####  4.3 渠道管理 API <a id="channel-api"></a>

此相关部分API可以进行渠道的ID查询及新建渠道。 

POST `https://www.growingio.com/api/v1/projects/{项目编号}/meta/channels` 

Request:

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| name | String | 名字 | 二维码推广 |

示例：

```text
{  "name":"线下地推"}
```

GET `https://www.growingio.com/api/v1/projects/{项目编号}/meta/channels` 

Response: Status Code: 200 OK

| 字段名 | 字段格式 | 说明 | 示例 |
| :--- | :--- | :--- | :--- |
| id | String | channel id | gnPNkoWA |
| name | String | 名字 | 今日头条 |

Response 示例：

```text
[  {    "id": "gnPNkoWA",    "name": "二维码推广"  },  {    "id": "jinritoutiao",    "name": "今日头条"  }]
```

####  <a id="link-api"></a>

#### 4.4 链接创建 API <a id="link-api"></a>

**请求地址**

POST 

```text
https://www.growingio.com/api/v1/projects/:project_id/meta/minplinks
```

**请求参数**

路径参数：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">projectId</td>
      <td style="text-align:left">
        <p></p>
        <p>nxog09md</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>Headers：

| 参数名称 | 参数值 | 是否必须 | 示例 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| Content-Type | application/json | 是 |  |  |

Body:

| 名称 | 类型 | 是否必须 | 备注 |
| :--- | :--- | :--- | :--- |
| productId | string | 必须 | 对应微信小程序应用id，在 4.1 请求应用中获得 |
| buildQrCode | boolean | 必须 | 是否创建二维码 |
| redirectUrl | string | 必须 | 跳转链接 |
| campaignId | string | 必须 | 活动 id |
| channelId | string | 必须 | 渠道 id |
| utmMedium | string | 非必须 | utm medium 参数 |
| utmContent | string | 非必须 | utm content 参数 |
| utmTerm | string | 非必须 | utm term 参数 |
| comments | string | 非必须 | 备注 |
| codeType | string | 非必须 | 二维码类型，A码或C码 |
| name | string | 必须 | 名字 |

调用 A 接口创建 小程序码 的请求参数示例

```text
// 创建小程序广告监测链接，同时创建小程序码 (A码)// Request Payload{    "name": "minp-qrcode-test-001",    "productId": "3oL4DgoW",    "redirectUrl": "pages/list/list",    "channelId": "woVOxv92",    "campaignId": "39l1r3R2",    "utmMedium": "广告媒介",    "utmTerm": "免费试用",    "utmContent": "广告内容",    "comments": "这是注释",    "buildQrCode": true,    "codeType": "A"}
```

调用 C 接口创建 小程序码 的请求参数示例

```text
// 创建小程序广告监测链接，同时创建小程序二维码 （C码）// Request Payload{    "name": "minp-qrcode-test-002",    "productId": "3oL4DgoW",    "redirectUrl": "pages/list/list",    "channelId": "woVOxv92",    "campaignId": "39l1r3R2",    "utmMedium": "广告媒介",    "utmTerm": "免费试用",    "utmContent": "广告内容",    "comments": "这是注释",    "buildQrCode": true,    "codeType": "C"}
```

### 返回数据

| 名称 | 类型 | 是否必须 | 备注 | 其他信息 |
| :--- | :--- | :--- | :--- | :--- |
| id | string | 非必须 |  |  |
| linkId | string | 非必须 | 监测链接ID |  |
| name | string | 非必须 | 名字 |  |
| projectId | string | 非必须 | 项目 id |  |
| spn | string | 非必须 | spn |  |
| trackingUrl | string | 非必须 | GrowingIO 分配的追踪链接 |  |
| redirectUrl | string | 非必须 | 目标链接 |  |
| campaignId | string | 非必须 | 活动 id |  |
| campaignName | string | 非必须 | 活动 名称 |  |
| channelId | string | 非必须 | 渠道 id |  |
| channelName | string | 非必须 | 渠道名称 |  |
| utmMedium | string | 非必须 | utm medium 参数 |  |
| utmContent | null | 非必须 | utm content 参数 |  |
| utmTerm | null | 非必须 | utm term 参数 |  |
| comments | null | 非必须 | 备注 |  |
| status | string | 非必须 | 状态 |  |
| creatorId | string | 非必须 | 创建人 id |  |
| creatorName | string | 非必须 | 创建人名称 |  |
| updaterId | string | 非必须 | 更新人 id |  |
| updaterName | string | 非必须 | 更新人名称 |  |
| createdAt | number | 非必须 | 创建时间 |  |
| updatedAt | number | 非必须 | 更新时间 |  |

调用 A 接口创建 小程序码 的请求，返回参数示例

```text
// Response{    "id": "a9a84ZoB",    "linkId": "a9a84ZoB",    "name": "minp-qrcode-test-001",    "projectId": "GR4mj3PM",    "spn": "wx51cba5e78d4ef4d8",    "trackingUrl": "pages/list/list?aid=a9a84ZoB",    "redirectUrl": "pages/list/list",    "campaignId": "39l1r3R2",    "campaignName": "上线前测试",    "channelId": "woVOxv92",    "channelName": "csdn",    "utmMedium": "广告媒介",    "utmContent": "广告内容",    "utmTerm": "免费试用",    "comments": "这是注释",    "qrCode": "https://gta.growingio.com/buckets/uploads/files/81624/wxcode/A/1557038991079/wxcode.jpg?sign=QNV9UcW0i%2BLVbDj%2F59KXV5l0kVQtGN%2BwxRDDOLyQQWc%3D&expires=1557039291693",    "status": "activated",    "creatorId": "6LPdeoNl",    "creatorName": "Dingding",    "updaterId": "6LPdeoNl",    "updaterName": "Dingding",    "createdAt": 1557038990657,    "updatedAt": 1557038990657}
```

调用 C 接口创建 小程序码 的请求，返回参数示例

```text
//Response {    "id": "nPNWAaoW",    "linkId": "nPNWAaoW",    "name": "minp-qrcode-test-002",    "projectId": "GR4mj3PM",    "spn": "wx51cba5e78d4ef4d8",    "trackingUrl": "pages/list/list?aid=nPNWAaoW",    "redirectUrl": "pages/list/list",    "campaignId": "39l1r3R2",    "campaignName": "上线前测试",    "channelId": "woVOxv92",    "channelName": "csdn",    "utmMedium": "广告媒介",    "utmContent": "广告内容",    "utmTerm": "免费试用",    "comments": "这是注释",    "qrCode": "https://gta.growingio.com/buckets/uploads/files/81624/wxcode/C/1557039139065/wxcode.jpg?sign=NIWNlKpis743%2BJ%2FJbB3ObrqTHXi0OCRlWyfGD8ng%2BIQ%3D&expires=1557039439512",    "status": "activated",    "creatorId": "6LPdeoNl",    "creatorName": "Dingding",    "updaterId": "6LPdeoNl",    "updaterName": "Dingding",    "createdAt": 1557039138779,    "updatedAt": 1557039138779}
```

