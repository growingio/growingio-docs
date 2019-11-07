# 统计数据导出 API

* [1.看板数据信息 API](reporting-api.md#dashboard-api)
* [2.事件分析下载 API](reporting-api.md#chart-api)
* [3.漏斗分析下载API](reporting-api.md#funnel-api)
* [4.留存分析下载API](reporting-api.md#retention-api)
* [5.分群下载 API](reporting-api.md#segmentation-api)
* [4.规则逻辑 API 接口](reporting-api.md#rule-api)

注意：

* 本页 API 中的 project\_id、dashboard\_id、chart\_id 、funnel\_id、retention\_id字段，均可在项目页面url中找到，如："[https://www.growingio.com/admin/projects/nxog09md/dashboard/YoX28w7R](https://www.growingio.com/admin/projects/nxog09md/dashboard/YoX28w7R)"  中的 "nxog09md" 和 "YoX28w7R" 分别是 project\_id 和dashboard\_id。
* dashboard\_id的获取方式：
  * 第一种是在项目的url上获取。
  * 第二种是根据1.1获取看板列表的api根据project\_id获取所有看板信息。
* chart\_id、funnel\_id、retention\_id的获取方式：
  * 第一种是在项目页面的url中获取。
  * 第二种是根据1.2获取看板中的图表信息api，根据dashboard\_id获取当前看板的所有图表信息，返回的信息中会包含图表id、图表类型等信息。
* 在进行导出之前，请务必参考[“GrowingIO接口认证”文档](authentication.md)，完成接口认证获取 token 。
* 统计数据导出的延迟一般为 30 分钟，比如导出早上 8 点到 9 点之间的数据时，一般需要 9:30 才能统计完毕。另外，每天凌晨因为需要运行天级别的统计任务，此时前一天的统计数据大概有 3-4 小时的延迟，一般凌晨 4 点以后会统计完毕。

### 1.看板数据信息 API <a id="dashboard-api"></a>

#### 1.1 获取看板列表

{% api-method method="get" host="https://www.growingio.com" path="/projects/:project\_id/dashboards.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}
  获取当前项目下全部看板列表，按更新时间由近到远排序
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[  {    "id": "Dashboard Uid",    "name": "我的看板",    "type": "看板类型", // normal: 普通看板, realtimeV2: 实时看板    "createdAt": "2019-01-01",    "updatedAt": "2019-01-02",    "scope": "看板所属", // global: 全局, project: 项目, user: 个人    "updater": "Dashboard Last Updator",    "creator": "Dashboard Creator"  },  ...]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### 1.2 获取看板中的图表信息

{% api-method method="get" host="https://www.growingio.com" path="/projects/:project\_id/dashboards/:dashboard\_id.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="dashboard\_id" type="string" required=true %}
看板 id
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{  "id": "Dashboard Uid",  "name": "Dashboard Name",  "charts": [    {      "id": "Chart Uid",      "name": "Chart Name",      "createor": "Chart Creator",      "createdAt": "Created Time",      "resource_type": "chart"    },    {      "id": "Funnel Uid",      "name": "Chart Name",      "createor": "Chart Creator",      "createdAt": "Created Time",      "resource_type": "funnel"    }    {      "id": "Retention Uid",      "name": "Chart Name",      "createor": "Chart Creator",      "createdAt": "Created Time",      "resource_type": "retention"    }    ...  ]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



### 2.事件分析下载 API V2（2018-08-29更新） <a id="chart-api"></a>

获取事件分析数据（单图下载每秒限速 2 次）

{% api-method method="get" host="https://www.growingio.com" path="/v2/projects/:project\_id/charts/:chart\_id.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="chart\_id" type="string" required=true %}
事件分析（单图 ID）
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="interval" type="integer" required=false %}
数据粒度，3600000\(小时\)，86400000\(天\)，604800000\(周\), 2592000000\(月\)，默认为天
{% endapi-method-parameter %}

{% api-method-parameter name="endTime" type="integer" required=true %}
数据结束时间，unix 毫秒时间戳
{% endapi-method-parameter %}

{% api-method-parameter name="startTime" type="integer" required=true %}
数据起始时间，unix 毫秒时间戳
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{    "id":  "Chart  Uid",    "name":  "Chart  Name",    "startTime":  1462118400000,    "endTime":  1462118400000,    "interval":  86400000,    "aggregator": {    // 当大数字图时返回该字段        "values": [            27557,    // 本周期聚合值            25409     // 上周期聚合值        ]    }    "meta":  [        {  "name":  "目标用户",  "dimension":  true},        {  "name":  "城市",  "dimension":  true  },        {  "name":  "浏览器",  "dimension":  true  },        {  "name":  "Metric  1",  "metric":  true  },        {  "name":  "Metric  2",  "metric":  true  }    ],    "data":  [        //  线图        [目标用户,  timestamp,  metric1,  metric2],        [目标用户,  timestamp,  metric1,  metric2]​        //  横向柱图        [目标用户,  dimension_v1,  metric1],        [目标用户,  dimension_v2,  metric1]​        //  纵向柱图        [目标用户,  timestamp,  metric1,  metrics2],        [目标用户,  timestamp,  metric1,  metrics2]​        //  表格        [目标用户,  dimension1_v1,  dimension2_v1,  metric1,  metric2],        [目标用户,  dimension1_v2,  dimension2_v1,  metric1,  metric2]​        //  大数字        [目标用户,  timestamp,  metric1]        //  气泡图        [目标用户,  dimension1_v1,  dimension2_v1,  metric1,  metric2]    ]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

###  <a id="funnel-api"></a>

### 3.漏斗分析下载API <a id="funnel-api"></a>

获取漏斗分析数据（单图下载每秒限速 2 次）

{% api-method method="get" host="https://www.growingio.com" path="/v2/projects/:project\_id/funnels/funnel\_id.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="funnel\_id" type="string" required=true %}
漏斗分析（单图ID）
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
项目uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证Token，见API认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO分配的公匙，见API认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="conversionWindow" type="integer" required=false %}
转化周期
{% endapi-method-parameter %}

{% api-method-parameter name="endTime" type="integer" required=true %}
数据结束时间，unix毫秒时间戳
{% endapi-method-parameter %}

{% api-method-parameter name="startTime" type="integer" required=true %}
数据起始时间，unix毫秒时间戳
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{"id":  "Funnel Uid","name":  "Funnel Name","conversionWindow":  1,"startTime":  1571068800000,"endTime":  1572278399999,"interval":  86400000,"meta":  [    {"name":"目标用户","dimension":true},    {"name":"时间","dimension":true},    {"name":"总转化率","metric":true},    {"name":"第一步人数","metric":true},    {"name":"第一步转化率","metric":true},    ...    {"name":"最后一步人数","metric":true},    {"name":"最后一步转化率","metric":true}]"data":  [    [目标用户, 时间, 总转化率, 第一步人数, ... , 最后一步转化率],    [目标用户, 时间, 总转化率, 第一步人数, ... , 最后一步转化率],    ...]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

###  <a id="retention-api"></a>

### 4.留存分析下载 API <a id="retention-api"></a>

获取留存分析数据（单图下载每秒限速 2 次）

{% api-method method="get" host="https://www.growingio.com" path="/v2/projects/:project\_id/retentions/:retention\_id.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="retention\_id" type="string" required=true %}
留存分析（单图ID）
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
项目uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证Token，见API认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO分配的公匙，见API认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="range" type="string" required=false %}
范围 day（日留存） \| week（周留存） \| month（月留存）
{% endapi-method-parameter %}

{% api-method-parameter name="endTime" type="integer" required=true %}
数据结束时间，unix毫秒时间戳
{% endapi-method-parameter %}

{% api-method-parameter name="startTime" type="integer" required=true %}
数据起始时间，unix毫秒时间戳
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{    "id":  "Retention Uid",    "name":  "Retention Name",    "range":  "day",    "startTime":  1569686400000,    "endTime":  1572278399999,    "interval":  86400000,    "meta":  [        {"name":"目标用户","dimension":true},        {"name":"对比值","dimension":true},        {"name":"用户行为","dimension":true},        {"name":"时间","dimension":true},        {"name":"留存人数","metric":true},        {"name":"当日","metric":true},        {"name":"当日留存率","metric":true},        {"name":"次日","metric":true},        {"name":"次日留存率","metric":true},        {"name":"2日后","metric":true},        {"name":"2日后留存率","metric":true},        . . .        {"name":"29日后","metric":true},        {"name":"29日后留存率","metric":true}    ],    "data":  [        [目标用户,对比值,用户行为,时间,留存,...,29日后留存],        . . .,        [目标用户,对比值,用户行为,时间,留存,...,29日后留存]    ]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

###  <a id="segmentation-api"></a>

### 5.分群下载 API <a id="segmentation-api"></a>

#### 5.1 获取分群列表 <a id="resource"></a>

{% api-method method="get" host="https://www.growingio.com" path="/projects/:project\_id/segmentations.json" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[  {    "id": "Segmentation Uid",    "name": "Segmentation Name",    "userType": 'u',    "userNum": 1230,    "updatedAt": "2016-08-03"  },  {    "id": "Segmentation Uid",    "name": "Segmentation Name",    "userType": 'cs1',    "userNum": 1230,    "updatedAt": "2016-08-03"  },  ...]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### 5.2 获取特定分群的用户列表 <a id="resource"></a>

{% api-method method="get" host="https://www.growingio.com" path="/projects/:project\_id/segmentations/:segmentation\_id/users.csv" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="segmentation\_id" type="string" required=true %}
分群 id
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
以 Tab 分割的 csv 文件，内容为上传的用户属性
{% endapi-method-response-example-description %}

```
cs1    name    12249    GrowingIO
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

\*\*\*\*

### 6.规则逻辑 API 接口 <a id="rule-api"></a>

获取圈选元素定义

{% api-method method="get" host="https://www.growingio.com" path="/projects/:project\_id/rules.csv" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
项目 uid
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token，见 API 认证文档
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，见 API 认证文档
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
以 Tab 分割的 csv 文件
{% endapi-method-response-example-description %}

```
ruleId,eventName,eventTypef2503720,元素_注册按钮,clck
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{  "message": "Unauthorized",  "errors": []}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{  "message": "Request timeout",  "errors": [    {      "code": "request_timeout",      "message": "Request timeout in 5000 milliseconds"    }  ]}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



