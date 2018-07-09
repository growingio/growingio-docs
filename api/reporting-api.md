# 统计数据导出 API

* [1.看板数据信息 API](reporting-api.md#dashboard-api)
* [2.单图数据下载 API](reporting-api.md#chart-api)
* [3.分群下载 API](reporting-api.md#segmentation-api)
* [4.规则逻辑 API 接口](reporting-api.md#rule-api)

###  {#dashboard-api}

注意：

* 本页 API 中的 project\_id、dashboard\_id、chart\_id 字段，均可在项目页面url中找到，如："[https://www.growingio.com/admin/projects/nxog09md/dashboard/YoX28w7R](https://www.growingio.com/admin/projects/nxog09md/dashboard/YoX28w7R)"  中的 "nxog09md" 和 "YoX28w7R" 分别是 project\_id 和dashboard\_id。
* 在进行导出之前，请务必参考[“GrowingIO接口认证”文档](authentication.md)，完成接口认证获取 token 。

### 1.看板数据信息 API {#dashboard-api}

获取看板中的图表信息

**1.1 Resource**

GET [https://www.growingio.com/projects/:project\_id/dashboards/:dashboard\_id.json](https://www.growingio.com/projects/:project_id/dashboards/:dashboard_id.json)

#### 1.2 Authorization {#authorization}

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在 GrowingIO 后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

#### 1.3 Response {#response}

Status Code: 200 OK

```text
{
  id: "Dashboard Uid",
  name: "Dashboard Name",
  charts: [
    {
      id: "Chart Uid",
      name: "Chart Name",
      createor: "Chart Creator",
      createdAt: "Created Time"
    },
    {
      id: "Chart Uid",
      name: "Chart Name",
      createor: "Chart Creator",
      createdAt: "Created Time"
    }
  ]
}
```

### 2.单图数据下载 API {#chart-api}

获取单图数据（单图下载每秒限速 2 次）

#### 2.1 Resource {#resource}

GET [https://www.growingio.com/projects/:project\_id/charts/:chart\_id.json](https://www.growingio.com/projects/:project_id/charts/:chart_id.json)

#### 2.2 Authorization {#authorization}

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

#### 2.3 Query Parameter {#query-parameter}

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- | --- |
| startTime | integer | 数据起始时间 | 1462118400000 |
| endTime | integer | 数据结束时间 | 1462118400000 |
| interval | integer | 数据间隔 | 3600000 \(小时\) 86400000 \(天\) 604800000 \(周\) 2592000000 \(月\) |

#### 2.4 Responses {#responses}

Status Code: 200 OK

```text
{
  id: "Chart Uid",
  name: "Chart Name",
  startTime: 1462118400000,
  endTime: 1462118400000,
  interval: 86400000,
  meta: [
    { name: '城市', dimension: true },
    { name: '浏览器', dimension: true },
    { name: 'Metric 1', metric: true },
    { name: 'Metric 2', metric: true }
  ],
  data: [
    // 线图
    [timestamp, metric1, metric2],
    [timestamp, metric1, metric2]

    // 横向柱图
    [dimension_v1, metric1],
    [dimension_v2, metric1]

    // 纵向柱图
    [timestamp, metric1, metrics2],
    [timestamp, metric1, metrics2]

    // 表格
    [dimension1_v1, dimension2_v1, metric1, metric2],
    [dimension1_v2, dimension2_v1, metric1, metric2]

    // 大数字
    [metric1]
  ]
}
```

### 3.分群下载 API {#segmentation-api}

#### 3.1 获取分群列表 {#resource}

**3.1.1  Resource** 

GET [https://www.growingio.com/projects/:project\_id/segmentations.json](https://www.growingio.com/projects/:project_id/segmentations.json)

**3.1.2 Authorization**

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

**3.1.3  Parameter**

无

**3.1.4 Responses**

Status Code: 200 OK

```text
[
  {
    id: "Segmentation Uid",
    name: "Segmentation Name",
    userType: 'u',
    userNum: 1230,
    updatedAt: "2016-08-03"
  },
  {
    id: "Segmentation Uid",
    name: "Segmentation Name",
    userType: 'cs1',
    userNum: 1230,
    updatedAt: "2016-08-03"
  },
  ...
]
```

#### 3.2 获取特定分群的用户列表 {#resource}

**3.2.1 Resource**

GET [https://www.growingio.com/projects/:project\_id/segmentations/:segmentation\_id/users.csv](https://www.growingio.com/projects/:project_id/segmentations/:segmentation_id/users.csv)

**3.2.2 Authorization**

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

**3.2.3 Query Parameter**

无

**3.2.4 Responses**

Status Code: 200 OK  
CSV 文件以 Tab 分隔，内容是上传的 CS 属性字段

```text
cs1_name    cs2_name    
12249    GrowingIO
```

### 4.规则逻辑 API 接口 {#rule-api}

获取圈选元素的规则

#### 4.1 Resource {#resource}

GET [https://www.growingio.com/projects/:project\_id/rules.csv](https://www.growingio.com/projects/:project_id/rules.csv)

#### 4.2 Authorization {#authorization}

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

#### 4.3 Query Parameter {#query-parameter}

无

#### 4.4 Responses {#responses}

Status Code: 200 OK  
CSV 文件内容以 Tab 分隔

```text
ruleId,eventName,eventType
f2503720,元素_注册按钮,clck
```

Status Code: 401 Unauthorized

```text
{
  "message": "Unauthorized",
  "errors": []
}
```

Status Code: 500 Server Error

```text
{
  "message": "Request timeout",
  "errors": [
    {
      "code": "request_timeout",
      "message": "Request timeout in 5000 milliseconds"
    }
  ]
}
```



