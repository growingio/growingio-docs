# 原始数据导出 2.0 API

* [1.原始数据导出 2.0 API 功能概要](raw-data-export-2.0.md#summary)
* [2.原始数据导出 2.0 API 接口定义](raw-data-export-2.0.md#definition)
  * [GET 按类型导出原始数据](raw-data-export-2.0.md#an-lei-xing-dao-chu-yuan-shi-shu-ju)
  * [GET 导出全部类型原始数据](raw-data-export-2.0.md#dao-chu-quan-bu-lei-xing-yuan-shi-shu-ju)
* [3.原始数据导出版本和GrowingIO数据主版本（SDK 版本）关系](raw-data-export-2.0.md#sdk-explaination)
* [4.原始数据导出 2.0 和原始数据导出 1.0 主要区别](raw-data-export-2.0.md#changelog)
* [5.原始数据导出 2.0 导出数据字段说明](raw-data-export-2.0.md#metadata)
  * [visit](raw-data-export-2.0.md#metadata_visit)
  * [page](raw-data-export-2.0.md#metadata_page)
  * [action](raw-data-export-2.0.md#metadata_action)
  * [action\_tag](raw-data-export-2.0.md#metadata_action_tag) /  [rules](raw-data-export-2.0.md#metadata_rule)
  * [custom\_event](raw-data-export-2.0.md#metadata_custom_event)
  * [pvar](raw-data-export-2.0.md#metadata_pvar)
  * [evar](raw-data-export-2.0.md#metadata_evar)
  * [ads\_track\_click](raw-data-export-2.0.md#metadata_ads_click)
  * [ads\_track\_activation](raw-data-export-2.0.md#metadata_ads_activation)
* [6.关于接口版本1.0升级到2.0注意事项](raw-data-export-2.0.md#upgrade)

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
导出数据类型，系统支持以下数据类型的导出，可选值：  
  
\* visit: 访问事件  
\* page: 页面事件  
\* action: 动作事件，包括点击、修改等动作  
\* action\_tag: 动作事件与圈选规则关联关系  
\* custom\_event: 自定义事件  
\* ads\_track\_activation: 广告激活事件  
\* ads\_track\_click: 广告点击事件  
\* pvar: 页面级变量  
\* evar: 转化变量
{% endapi-method-parameter %}

{% api-method-parameter name="ai" type="string" required=true %}
项目 ID
{% endapi-method-parameter %}

{% api-method-parameter name="export\_date" type="string" required=true %}
导出数据北京时间，格式为 yyyyMMddHHmm, 表现请求导出哪段时间内的数据，分为以下情况：  
  
\* 当 export\_type 为 day 时，只会截取 export\_date 中 yyyyMMdd，其余将忽略  
\* 当  export\_type 为 hour 时，只会截图 export\_date 中 yyyyMMddHH，其余将忽略
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，请在 GrowingIO 后台项目管理页面获得。示例：'X-Client-Id: 123abc'
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
 认证后获取到的 Token，示例: 'Authorization: Token XXXX'
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="minutes" type="integer" required=false %}
 链接失效时间，单位为分钟，默认为 5
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
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
\* 当 export\_type 为 hour 时，只会截图 export\_date 中 yyyyMMddHH，其余将忽略
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥，请在 GrowingIO 后台项目管理页面获得。示例：'X-Client-Id: 123abc'
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
认证后获取到的 Token，示例: 'Authorization: Token XXXX'
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

```javascript
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
* downloadLinks 表示文件的下载链接，

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

### 4.原始数据导出 2.0 和原始数据导出 1.0 主要区别 {#changelog}

#### 接口请求 URL {#接口请求-url}

原始数据导出 1.0 接口格式：

`https://www.growingio.com/insights/:ai/:date.json`

原始数据导出 2.0 接口格式：

`https://www.growingio.com/v2/insights/{export_type}/{data_type}/{ai}/{export_date}.json`

#### 接口响应数据格式 {#接口响应数据格式}

原始数据导出 1.0 接口返回数据格式：

```text
{
  "status":"FINISHED",
  "downlinks":[
    "link1",
    "link2"
  ]
}
```

原始数据导出 2.0 接口返回数据格式：

```text
{
  "status”:"FINISHED",
  "downloadLinks":[
    "link1",
    "link2"
  ],
  "exportType":"",
  "dataType":"",
  "exportDate":"",
  "exportVersion":"",
  "requestTime":"",
  "errorMsg":""
}
```

#### 数据文件的获取方式和组织 {#数据文件的获取方式和组织}

原始数据导出 1.0

1. 按天和小时的维度区分获取数据，一次获取所有类型的数据
2. 天数据一次返回所有7种类型当天的7个数据文件
3. 小时数据一次返回所有7种类型当天特定小时的7个数据文件

原始数据导出 2.0

1. 按导出类型（天或小时）和数据类型（v2共9种）区分获取数据，一次获取一种类型特定时间的数据
2. 天数据返回特定日期所有小时的数据文件
3. 小时数据根据确定的数据类型和时间获取确定类型确定小时的1个数据文件

#### 原始数据导出提供的数据类型 {#原始数据导出提供的数据类型}

原始数据导出 1.0 提供的数据类型：

visit、page、action、action\_tag、custom\_attr、ads\_track\_click、ads\_track\_activation一共7种类型的数据

原始数据导出 2.0 提供的数据类型：

visit、page、action、action\_tag、custom\_event（原custom\_attr）、ads\_track\_click、ads\_track\_activation、pvar（新增）、evar（新增）一共9种类型的数据

#### 数据格式变化 {#数据格式变化}

visit：新增字段，部分字段名称变化

page：新增字段，部分字段名称变化

action：部分字段名称变化

action\_tag：字段名称变化

ads\_track\_activation：部分字段名称变化

custom\_event：原custom\_attr，新增字段，部分字段名称变化

pvar：新增类型

evar：新增类型

### 5.原始数据导出 2.0 导出数据字段说明 {#metadata}

#### page请求导出字段 {#metadata_page}

#### vst请求导出字段 {#metadata_visit}

API1.0接口中的**`countryName、region、city`**三个字段，在API2.0接口中已经删除。原因是这三个字段实际由GrowingIO内部库通过**`解析IP、地理位置`**得到的结果，可能与客户自己解析出来的结果存在差异，这样会造成客户识别的困扰。

#### action请求导出字段 {#metadata_action}

#### cstm请求导出字段 {#metadata_custom_event}

#### pvar请求导出字段 {#metadata_pvar}

#### evar请求导出字段 {#metadata_evar}

#### action\_tag导出字段 {#metadata_action_tag}

#### rules（从[统计数据的规则逻辑API](https://docs.growingio.com/docs/api/reporting-api#rule-api)获取） {#metadata_rule}

1. 在基础部分数据导出（visit，page，action）之外，提供圈选数据与action级别数据的映射部分。
2.  **rules**表示客户在GrowingIO平台上圈选的标签，rule\_id是其唯一标识。
3.  通过**action**中的`action_id`与**action\_tag**中的`action_id`聚合，然后绑定**action\_tag**中的`rule_id`到**rules**中对应的`rule_id、name`到action的数据上。这样就可以通过规则名称进行数据分析，识别导出数据中圈选部分的数据情况。
4. 建议规则建立时保持名称的唯一性，GrowingIO平台不保证规则名称的唯一。
5. 相同的名称下可能有多个规则类型，`规则名称+规则类型`才能区分开，此处类型与基础数据**action**中的`请求事件类型`保持一致。

#### ads\_track\_activation 请求导出字段 {#metadata_ads_activation}

#### ads\_track\_click 请求导出字段 {#metadata_ads_click}

### 6.关于接口版本1.0升级到2.0注意事项 {#upgrade}

如果您目前使用的API版本是1.0，想要升级为2.0，请联系客户成功经理。如果您已经从1.0升级到了2.0，请注意如下事项：

* 从API2.0接口开通那一刻起，API1.0接口依旧可以继续使用，但是**`一个月`**之后就会失效，请您在过渡期内尽快升级您的使用代码。
* API2.0接口从开通的那一刻起就开始生效，比如您于`2018-08-01 10:30:00`升级到了API2.0接口，那么API2.0接口就可以下载`2018-08-01 11:00:00`之后的数据了，但是之前的数据还需要通过API1.0去获取。



