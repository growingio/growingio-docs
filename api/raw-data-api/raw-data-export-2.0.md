# 原始数据导出 2.0 API

* [1.原始数据导出 2.0 API 功能概要](raw-data-export-2.0.md#summary)
* [2.原始数据导出 2.0 API 接口定义](raw-data-export-2.0.md#definition)
  * [GET 按类型导出原始数据](raw-data-export-2.0.md#an-lei-xing-dao-chu-yuan-shi-shu-ju)
  * [GET 导出全部类型原始数据](raw-data-export-2.0.md#dao-chu-quan-bu-lei-xing-yuan-shi-shu-ju)
* [3.原始数据导出版本和GrowingIO数据主版本（SDK 版本）关系](raw-data-export-2.0.md#sdk-explaination)



原始数据导出为付费功能，且只能导出从开通之日起的原始数据，原始数据仅保留15天，请定期下载。数据导出一般延迟为 30 分钟，比如早上 8 点到 9 点之间的数据，一般 9:30 会准备好。每天凌晨因为需要运行天级别的统计任务，所以导出任务会延迟 1-2 小时，在导出数据时请判断  `status` 字段 。

导出时数据以每 64M 为单位分包发送，导出数据默认采用 gzip 压缩。原始数据中所有时间字段均为 [UTC](http://baike.baidu.com/link?url=T9ER87o8wd_ABq-oRrn839-Q2hxrV5WvIeQX2bJCOAWgne8C8BCw8yRWrISceZJEoR83GuIhdu0vSZFwzl4ngFrD7vUITsrlcY6U3Fj6lWCx7x0xWRTNDFOHkhJmnUW05hrb5df7vvz12EayMr_4b5QJZ1UcTs17ffae3wI18LNeF8j_4WpMZ_srcJHSXhpk) 时间，并非中国时间；此处导出的压缩包名也是由 UTC 时间命名。

在进行原始数据导出之前，请务必参考 [“GrowingIO接口认证”文档](https://docs.growingio.com/api/authentication.html)，完成接口认证获取 token 。

### 1.原始数据导出 2.0 API 功能概要 {#summary}

1. “2.0版”提供了小时级别的原始数据导出。

   例如，下午1点到2点的原始数据，在一般情况下可以在3点准备好。

2. “2.0版”在包含所有“1.0版”具有的数据表和字段的基础上添加了数据主版本2.0（SDK 2.x）版本具有的自定义事件和变量的数据表和字段，并对包括广告监测数据导出在内的所有数据导出表的字段做了统一处理。
3. “2.0版”兼容并包含了"1.0版"的所有数据字段。

### 2.原始数据导出 2.0 API 接口定义 {#definition}

{% api-method method="get" host="https://www.growingio.com" path="/v2/insights/:export\_type/${data\_type}/${ai}/${export\_date}.json?expire=${minutes}" %}
{% api-method-summary %}
按类型导出原始数据
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="export\_type" type="string" required=true %}
导出任务类型，系统目前支持小时与天的导出，可选值：hour 或者 day
{% endapi-method-parameter %}

{% api-method-parameter name="data\_type" type="string" required=true %}
导出数据类型，系统支持一下数据类型的导出，可选值：  
  
\* visit: 访问事件  
\* page: 页面事件  
\* action: 动作事件，包括点击、修改等动作  
\* action\_tag: 动作事件与圈选规则关联关系  
\* custom\_event: 自定义事件  
\* ads\_track\_activation: 广告激活事件  
\* ads\_track\_click: 广告点击事件  
\* pvar:页面级变量  
\* evar: 转化变量 
{% endapi-method-parameter %}

{% api-method-parameter name="ai" type="string" required=true %}
项目 ID
{% endapi-method-parameter %}

{% api-method-parameter name="export\_date" type="string" required=true %}
导出数据北京时间，格式为 yyyyMMddHHmm, 表现请求导出哪段时间内的数据，分为以下情况：  
  
\* 当 export\_type 为 day 时，只会截取 export\_date 中 yyyyMMdd, 其余将忽略  
\* 当 export\_type 为 hour 时，只会截取 export\_date 中 yyyyMMddHH, 其余将忽略
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，请在 GrowingIO 后台项目管理页面获得。示例： 'X-Client-Id: 123abc'
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
认证后获取到的 Token，示例：'Authorization: Token XXXX  
'
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="minutes" type="integer" required=false %}
链接失效时间，单位为分钟，默认为5
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "status": "",
  "downloadLinks": [],
  "exportType": "",
  "dataType": "",
  "exportDate": "",
  "exportVersion": "",
  "requestTime": "",
  "errorMsg": ""
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://www.growingio.com" path="/v2/insights/${export\_type}/${ai}/${export\_date}.json?expire=${minutes}" %}
{% api-method-summary %}
导出全部类型原始数据
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="export\_type" type="string" required=true %}
导出任务类型，系统目前支持小时与天的导出，可选值：hour 或者 day
{% endapi-method-parameter %}

{% api-method-parameter name="ai" type="string" required=true %}
项目 ID
{% endapi-method-parameter %}

{% api-method-parameter name="export\_date" type="string" required=true %}
导出数据北京时间，格式为 yyyyMMddHHmm, 表现请求导出哪段时间内的数据，分为以下情况：  
  
\* 当 export\_type 为 day 时，只会截取 export\_date 中 yyyyMMdd，其余将忽略  
\* 当 export\_type 为 hour 时，只会截取 export\_date 中 yyyyMMddHH，其余将忽略
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，请在 GrowingIO 后台项目管理页面获得。示例：'X-Client-Id: 123abc'
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
认证后获取到的 Token，示例：'Authorization: Token XXXX'
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="minute" type="string" required=false %}
链接失效时间，单位为分钟，默认为 5
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "status": "FINISHED",
    "downloadLinks": {
        "evar": [],
        "pvar": [],
        "action_tag": [],
        "custom_event": [],
        "page": [],
        "ads_track_activation": [],
        "visit": [],
        "ads_track_click": [],
        "action": []
    },
    "exportType": "hour",
    "exportDate": "2018070100",
    "exportVersion": "v2",
    "requestTime": "2018-07-18 02:37",
    "errorMsg": ""
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

返回结果中的字段含义为：

* status 表示导出状态，可能值为：
  * FINISHED：导出任务完成，客户可以使用 downloadLinks 中的链接进行文件下载
  * RUNNING：导出任务正在运行
  * NOT\_EXISTS：导出任务不存在，请检查是否开启了导出功能并检查请求日期时间
  * ERROR：导出任务发生错误
* downloadLinks 表示文件的下载链接，key为数据类型，参考： [原始数据导出字段说明](fields/)

  **链接可能随时调整，不应使用链接中任何内容作为处理程序依据，链接默认过期时间为五分钟**

* exportType 表示导出任务类型，由用户传入，服务器原样返回
* dataType 表示导出数据类型，由用户传入，服务器原样返回
* exportDate 表示导出数据时间，由用户传入，服务器根据 exportType 截取后返回
* exportVersion 表示导出数据版本，当前版本下默认为 v2
* requestTime 客户请求发生时服务器时间
* errorMsg 当请求发生错误时，服务器返回的错误信息

### 3.原始数据导出版本和GrowingIO数据主版本（SDK 版本）关系 {#sdk-explaination}

![](https://docs.growingio.com/.gitbook/assets/datafeed.png)

在“原始数据导出 2.0 API” 上线之前，使用数据主版本2的客户也在使用“原始数据导出 1.0 API”。在“原始数据导出 1.0 API”版本中并没有包括如：页面级变量、转化变量这样的原始数据。在“原始数据导出 2.0 API”中提供了这部分原始数据的导出功能。

