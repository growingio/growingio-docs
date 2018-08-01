# 原始数据导出 1.0 API（已废弃）

**注意：原始数据导出 2.0 API 已经上线，如果原始数据导出功能需要升级到 2.0 版本请联系客户成功经理，并请及时按照**[**原始数据导出 2.0 API**](https://docs.growingio.com/api/raw-data-export-2.0.html)**的字段格式更新原始数据的 ETL 脚本程序。**

**如您使用的是 2.x 版本的 SDK，请务必使用**[ **2.0 版本的原始数据导出的 API**](https://docs.growingio.com/api/yuan-shi-shu-ju-dao-chu-2.0-api.html)。

1. 导出时数据以每 64M 为单位分包发送，导出数据默认采用 gzip 压缩。原始数据中所有时间字段均为 [UTC](http://baike.baidu.com/link?url=T9ER87o8wd_ABq-oRrn839-Q2hxrV5WvIeQX2bJCOAWgne8C8BCw8yRWrISceZJEoR83GuIhdu0vSZFwzl4ngFrD7vUITsrlcY6U3Fj6lWCx7x0xWRTNDFOHkhJmnUW05hrb5df7vvz12EayMr_4b5QJZ1UcTs17ffae3wI18LNeF8j_4WpMZ_srcJHSXhpk) 时间，并非中国时间；此处导出的压缩包名也是由 UTC 时间命名。
2. 原始数据导出为付费功能，且只能导出从开通日起的数据，原始数据仅保留15天，请定期下载。
3. 在进行导出之前，请务必参考[“GrowingIO接口认证”文档](https://docs.growingio.com/api/authentication.html)，完成接口认证获取token。

## 1.Resource

GET `https://www.growingio.com/insights/:ai/:date.json`

### 1.1 Authorization

在 Header 里面添加两个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |
| Authorization | String | 认证后获取到的 Token | Authorization: Token xxxxxx |

### 1.2 URL Parameter

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- | --- |
| ai | String | 项目 ID | 2a1b4018cd954ec2bcc69da5138bdb96 |
| date | String | 具体数据日期 | 20160520 |
| expire | Integer | 过期时间, 以分为单位 | 5 |

date分为两种格式：精确到天\(20160520\)或者精确到小时\(2016052008,20日早上8点\)，分别对应下载天级别数据与小时级别数据。均为中国时间，不是UTC时间。

### 1.3 Responses

Status Code: 200 OK

```text
{
  "status": "FINISHED",
  "downlinks": [
    "link1",
    "link2"
  ]
}
```

响应中的 status 字段，状态值有： 1. FINISHED 任务完成； 2. RUNNING 任务正在跑； 3. NOT\_EXISTS 任务不存在，可能是任务还没跑或者请求日期格式不对。

使用下载接口时，可以先检查 status 字段，为 FINISHED 时才拿 downlinks 进行下载。

### 2.导出数据字段说明

GrowingIO全量数据划分成三个级别，visit，page，action

#### 2.1 visit

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值（example） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| userId | 用户ID | string\(36\) | 针对单个用户生成的唯一id | 例如，web网站生成一个有效期三年的cookie值，App 则为机器唯一标识码 |
| sessionId | 会话ID | string\(36\) | "web: 30分钟过期的session值，代表一次会话。mobile: app退出30秒后再进入，刷新session值" |  |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 |  |
| eventTime | 事件发生时间 | bigint | 事件实际发生的时间 |  |
| eventType | 事件类型 | string\(10\) | 该消息的类型 | visit表内消息类型为vst |
| ip | ip | string\(64\) | 用户的网络ip地址 | 10.10.0.21 |
| countryName | 国家名称 | string\(30\) | 用户所在国家 | 中国 |
| region | 省份 | string\(30\) | 用户所在省份区域 | 浙江省 |
| city | 城市 | string\(30\) | 用户所在城市 | 杭州市 |
| domain | 域名 | string\(100\) | 用户访问的网站域名 | www.growingio.com |
| path | 路径 | string\(512\) | 网站路径 | /login |
| refer | 来源链接 | string\(1024\) | 该用户从refer所在地址跳转过来 |  |
| userAgent | 用户客户端 | string\(1024\) | 简称ua，例如浏览器信息或者移动设备信息 |  |
| appVersion | app版本 | string\(10\) | 客户的产品版本，仅限App端 |  |
| model | 设备型号 | string\(50\) | 用户的设备型号，例如小米4 |  |
| manufacturer | 设备厂商 | string\(50\) | 用户的设备生产厂商，例如小米 |  |
| channel | 下载渠道 | string\(40\) | app的下载渠道，仅限mobile |  |
| language | 语言 | string\(10\) | 用户使用的设备系统语言 | cn, en |
| osVersion | 系统版本 | string\(50\) | 用户使用的设备系统版本 | ios 9.1.3 |
| resolution | 设备分辨率 | string\(20\) | 用户使用的设备分辨率 | 1600 \* 1800 |
| platform | 数据来源平台 | string\(10\) | 区分该数据属于哪个平台 | web, android, ios |
| id | 访问事件id | string\(16\) | 即visit\_id，用于与page数据聚合 | 唯一标记visit事件 |
| query | 访问事件的query信息 | string\(512\) | 访问时的链接中的query，与前面的domain，path一起构建完整的链接 | 可用于提取utm等字段 |
| lat | gps纬度 | double | mobile平台，需要gps权限 |  |
| lng | gps经度 | double | mobile平台独有的字段 | 29.43982，精确到小数点后5位 |

#### 2.2 page

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值（example） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| userId | 用户ID | string\(36\) | 针对单个用户生成的唯一id | 例如，web网站生成一个有效期三年的cookie值，App 则为机器唯一标识码 |
| sessionId | 会话ID | string\(36\) | "web: 30分钟过期的session值，代表一次会话。mobile: app退出30秒后再进入，刷新session值" |  |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 |  |
| eventTime | 事件发生时间 | bigint | 事件实际发生的时间 |  |
| eventType | 事件类型 | string\(15\) | 该消息的类型 | page表内类型为page |
| domain | 域名 | string\(100\) | 用户访问的网站域名 | www.growingio.com |
| path | 路径 | string\(512\) | 网站路径 | /login |
| query | 参数 | string\(512\) | request请求中的查询参数 | k1=v1&k2=v2 |
| refer | 来源链接 | string\(1024\) | 该用户从refer所在地址跳转过来 |  |
| title | 页面名称 | string\(1024\) | 该网页名称，page title |  |
| platform | 数据来源平台 | string\(10\) | 区分该数据属于哪个平台：web, android, ios |  |
| cs1 | 用户信息字段1 | string\(200\) | 客户平台的登陆用户id：cs1，如果客户安装sdk时设置过cs1字段（上传用户属性字段集cs，cs1用于设置用户id） | userid:12345 |
| cs2 | ... | string\(200\) | 客户平台的项目id：cs2 | projectid:abc |
| cs3 | ... | string\(200\) |  |  |
| cs4 | ... | string\(200\) |  |  |
| cs5 | ... | string\(200\) |  |  |
| cs6 | ... | string\(200\) |  |  |
| cs7 | ... | string\(200\) |  |  |
| cs8 | ... | string\(200\) |  |  |
| cs9 | ... | string\(200\) |  |  |
| cs10 | ... | string\(200\) |  |  |
| id | 页面事件id | string\(23\) | 即page\_id，用于与action数据聚合 | 唯一标记page事件 |
| visit\_id | 访问事件id | string\(16\) | 即visit\_id，用于与visit表数据聚合 | visit表内id |
| pagegroup | 页面组名称 | string\(100\) | 页面组名称，需要在sdk集成时配置 | 用于标记设置的一组ps字段信息 |
| ps1 | 页面信息字段1 | string\(200\) | sdk配置的页面信息字段1 |  |
| ps2 | ... | string\(200\) |  |  |
| ps3 | ... | string\(200\) |  |  |
| ps4 | ... | string\(200\) |  |  |
| ps5 | ... | string\(200\) |  |  |
| ps6 | ... | string\(200\) |  |  |
| ps7 | ... | string\(200\) |  |  |
| ps8 | ... | string\(200\) |  |  |
| ps9 | ... | string\(200\) |  |  |
| ps10 | ... | string\(200\) |  | ~ |

#### 2.3 action

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值（example） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| userId | 用户ID | string\(36\) |  |  |
| sessionId | 会话ID | string\(36\) | "web: 30分钟过期的session值，代表一次会话,。mobile: app退出30秒后再进入，刷新session值" |  |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 |  |
| eventTime | 事件发生时间 | bigint | 事件实际发生的时间 |  |
| eventType | 事件类型 | string\(10\) | 该消息的类型 | 可能值为clck\(click\), chng\(change\)，sbmt\(submit\)以及imp\(impression\)，change的含义 |
| eventValue | 事件值 | string\(1024\) | 该消息的值，例如标签的value | “确定” |
| domain | 域名 | string\(100\) | 用户访问的网站域名 | www.growingio.com |
| path | 路径 | string\(512\) | 网站路径 | /login |
| href | 链接 | string\(1024\) | 标签内的跳转链接（如果没有则为null） |  |
| page\_id | 页面ID | string\(23\) | 页面唯一的id，用于与page数据join |  |
| action\_id | 事件ID | string\(30\) | 标签事件的唯一id | web的action\_id以wa开头，mobile以ma开头 |
| index | 列表序号 | bigint | 列表类型标签的序号 | 用于标记列表内的第几项，分析列表中最常被点击的内容或者首项推广效果等等 |
| info | 事件附加信息 | string\(200\) | 用户自定义事件信息 | 对应growingAttributesInfo设置的字段信息 |

### 3.字段备注

1. 三张数据表分别代表GIO定义的三种数据级别，访问级别（visit），页面级别（page）与标签级别（action）。visit代表访问级别的数据，按照session定义访问，page代表页面级别数据，打开的浏览页面就是一条记录，一条访问级别数据对应多条页面级别，action级别数据代表标签数据，定义页面元素标签的显示，点击，提交等事件，三者形成整个用户行为数据层级。目前导出的数据类型除了action下的imp\(impression\)类型因为数据量过大不可导出，其它数据都已经导出。
2. sendTime与eventTime的区别在于前者相当于是GIO平台接收到的时间，而eventTime是事件在客户端真正发生的时间，客户可以根据eventTime重现用户操作时间线。
3. 在refer中可以提取utm（广告链接关键字）或者搜索关键字等信息，用于分析访问来源。也可在visit表的query字段中提取utm信息。
4. appVersion，model，manufacturer，channel，osVersion仅在mobile端提供，更多信息可以从userAgent中提取。
5. 三张数据表可以根据“外键”join，分别是page\_id与page表的id，visit\_id与visit表的id，action\_id单独提供。因为标签事件并不导出impression（显示级别）的数据（数据量太大的缘故），所以建议通过action full outer join page，visit与page基本保持对应，若是在小时级别page数据无法join到对应的visit记录，visit记录可能存在于之前的小时单位中。
6. 所有数据已经根据userId, sessionId, sendTime进行排序，基本能够做到具体用户行为跟踪。
7. mobile端浏览器打开页面访问，默认platform类型为Web，若是需要区分则建议根据osVersion。
8. action数据中index，info为补充字段，参考changelog说明。

**在全量数据之外，提供 GrowingIO 平台圈选数据映射关系表，包括 action\_tag 与 rules 表\(接口将更新于2016年8月9日\)**

#### 3.1 action\_tag {#actiontag}

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值\(example\) |
| --- | --- | --- | --- |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 |  |
| action\_id | 事件ID | string\(30\) | 标签事件的唯一id web的action\_id以wa开头，mobile以ma开头 |  |
| rule\_id | 规则id | string\(8\) | 匹配事件的规则id，该id为growingio平台圈选的标签的唯一id | 该值由字母与数字组成，例如‘1ba052a9’ |

#### 3.2 rules（从统计数据的规则逻辑API获取） {#rules（从统计数据的规则逻辑api获取）}

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值\(example\) |
| --- | --- | --- | --- |
| rule\_id | 规则id | string\(8\) | 匹配事件的规则id，该id为growingio平台圈选的标签的唯一id | 该值由字母与数字组成，例如‘1ba052a9’ |
| name | 规则名称 | string\(200\) | 圈选的标签名称 | 该名称不可以作为唯一主键，只是便于使用区分 |
| ruleType | 规则类型 | string\(10\) | 规则在定义时可能有不同的类型，例如按钮的imp或者clck | 值包括 page, imp, clck, chng, sbmt |

#### 3.3 备注 {#备注}

1. 在基础部分数据导出（visit, page, action\)之外，提供圈选数据与action级别数据的映射部分。
2. 通过action数据中的action\_id与action\_tag中的action\_id聚合，绑定对应的rule\_id（映射的规则名称）到action数据上。
3. rules代表了客户在GrowingIO平台上圈选的标签，rule\_id即其唯一标识符。
4. 通过rules表将名称绑定到上述的action\_tag表中，便于通过名称进行数据分析，识别导出数据中圈选部分的数据情况。
5. action\_tag与rules表均是关联信息表，用于更进一步分析导出的部分数据，在导出数据中定位圈选数据。建议规则建立时保持名称的唯一性，GrowingIO平台不保证规则名称唯一性。
6. 相同的规则名称下可能有多个规则类型，规则名称＋规则类型才能区分，此处的规则类型与基础数据action中的事件类型保持一致。

#### 打点数据 custom\_attr {#打点数据-customattr}

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值\(example\) |
| --- | --- | --- | --- |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 |  |
| eventname | 埋点事件名称 | string | 用户定义的埋点事件名称 | 管理平台内定义 |
| attribute | 埋点事件属性 | string | 埋点事件属性为json字符串 | 以`_` 开头的字段为growingio相关定义字段，包括\_sessionId，\_userId，\_domain，\_eventTime等，其他为客户自定义的字段 |

**custom\_attr 内 attribute 字段说明**

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值\(example\) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| \_userId | 用户ID | string\(36\) | 针对单个用户生成的唯一id | 例如，web网站生成一个有效期三年的cookie值，mobile则为机器唯一标识码 |
| \_sessionId | 会话ID | string\(36\) | "web: 30分钟过期的session值，代表一次会话。mobile: app退出30秒后再进入，刷新session值" |  |
| \_eventTime | 事件发生时间 | bigint | 事件实际发生的时间 |  |
| \_domain | 域名 | string\(100\) | 用户访问的网站域名 | www.growingio.com |
| \_path | 路径 | string\(512\) | 网站路径 | /login |
| \_query | 访问事件的query信息 | string\(512\) | 访问时的链接中的query，与前面的domain，path一起构建完整的链接 |  |
| \_cs1 | 自定义用户信息字段1 | string\(200\) | 客户平台的登陆用户id：cs1，如果客户安装sdk时设置过cs1字段（上传用户属性字段集cs，cs1用于设置用户id） | cs1默认用于标记登陆用户ID， userid:12345 |
| xxx | 用户打点字段 | string / double | 客户自定义的打点字段，这部分字段最多有30个字段 | 所有自定义的打点字段，不同的事件有不同的定义，需要客户根据eventName自定义解析处理 |

#### 广告监测API数据 {#广告监测api数据}

**激活日志 ads\_track\_activation**

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值（example） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| userId | 用户ID | string\(36\) | 针对单个用户生成的唯一id | 例如，web网站生成一个有效期三年的cookie值，App 则为机器唯一标识码 |
| idfa | idfa | string\(32\) | iOS系统中的设备标志 | CCD6E1CD-8C4B-40CB-8A62-4BBC7AFE07D6 |
| imei | imei | string\(32\) | 安卓系统中的设备唯一标志 | 10bc955ac2a67ed3 |
| uuid | uuid | string\(32\) | GIO在设备中成的设备标志ID | 略。即将下线，可忽视此字段 |
| androidid | androidid | string\(32\) | 安卓系统中的设备唯一标志 | 9774d56d682e549c |
| ip | ip | string\(64\) | 用户IP地址 | 10.10.0.21 |
| useragent | 用户客户端UA信息 | string\(1024\) | 简称ua，例如浏览器信息或者移动设备信息 | 略 |
| platform | 数据来源平台 | string\(10\) | 区分该数据属于哪个平台 | web, android, ios |
| osversion | 系统版本 | string\(50\) | 用户使用的设备系统版本 | ios 9.1.3 |
| sendTime | 发送时间 | bigint | 数据发送过来的时间 | 略 |
| link\_id | 监测链接ID | string\(10\) | 监测链接ID | o3ox2dV |
| campaign\_id | 推广活动ID | string\(10\) | 推广活动ID | 略 |
| channel\_id | 目标渠道ID | string\(20\) | 目标渠道ID | 略 |
| link\_name | 链接名称 | string | 链接名称 | 测试链接 |
| campaign\_name | 活动名称 | string | 活动名称 | 双十一推广 |
| channel\_name | 渠道名称 | string | 渠道名称 | 今日头条 |

**点击日志 ads\_track\_click**

| 列名 | 字段名称 | 字段格式 | 字段说明 | 值（example） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| idfa | idfa | string\(32\) | iOS系统中的设备标志 | CCD6E1CD-8C4B-40CB-8A62-4BBC7AFE07D6 |
| imei | imei | string\(32\) | 安卓系统中的设备唯一标志 | 10bc955ac2a67ed3 |
| uuid | uuid | string\(32\) | GIO在设备中成的设备标志ID | 略。即将下线，可忽视此字段 |
| androidid | androidid | string\(32\) | 安卓系统中的设备唯一标志 | 9774d56d682e549c |
| ip | ip | string\(64\) | 用户IP地址 | 10.10.0.21 |
| useragent | 用户客户端UA信息 | string\(1024\) | 简称ua，例如浏览器信息或者移动设备信息 | 略 |
| platform | 数据来源平台 | string\(10\) | 区分该数据属于哪个平台 | web, android, ios |
| eventTime | 事件发生时间 | bigint | 事件实际发生的时间 |  |
| link\_id | 监测链接ID | string\(10\) | 监测链接ID | o3ox2dV |
| campaign\_id | 推广活动ID | string\(10\) | 推广活动ID | 略 |
| channel\_id | 目标渠道ID | string\(20\) | 目标渠道ID | 略 |
| link\_name | 链接名称 | string | 链接名称 | 测试链接 |
| campaign\_name | 活动名称 | string | 活动名称 | 双十一推广 |
| channel\_name | 渠道名称 | string | 渠道名称 | 今日头条 |

**（老版）来源管理数据导出api**

GET [https://www.growingio.com/projects/project\_id/activities.json](https://www.growingio.com/projects/project_id/activities.json)

上述地址中的 project\_id 取值请参考[“GrowingIO接口认证”](https://docs.growingio.com/api/authentication.html#shu-yu)章节的“术语”。

**输出**

| 字段名称 | 示例 | 中文含义 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| id | a9a7jrPB | -- |
| name | 12.16增长大会 | 名称\(落地页命名\) |
| href | www.growingio.com | 目标链接 |
| platform | 网页为"web" 移动应用为"mobile" | 平台 |
| utmCampaign | 增长大会活动首页 | 广告名称 |
| utmMedium | CPC | 广告媒介 |
| utmSource | 广点通 | 广告来源 |
| utmContent | 活动介绍和报名表单 | 广告内容 |
| utmTerm | 增长Growingio | 广告关键词 |
| comment | 推广预算两万 | 备注 |
| advertiser | 普通推广为"none" | 推广平台 |
| projectId | 5138bdb96 | 项目ID |
| creatorName | Jacky | 创建人 |
| createdAt | 1484397370856 | 创建时间 |
| shortUrl | [https://s.growingio.com/6XNmKl](https://s.growingio.com/6XNmKl) | 用于投放的短链 |
| uv | 100 | 新用户，通过GIO广告链接访问广告，并在过去90天内首次使用您的App或网站的用户。用户首次使用后，数据有1小时左右的延迟。 |
| tc | 120 | 链接访问，GIO广告链接的访问次数，通常通过点击广告位或者扫码访问。用户访问后，数据有1小时左右的延迟。 |

### 附录 {#附录}

#### 数据处理建议 {#数据处理建议}

数据处理建议采用Hive或者Spark平台工具，若是需要导入自有BI平台，可能需要进一步调整数据格式（csv转成其他符合数据处理需求的格式），针对以上的需求，给出相应的数据处理建议。

_**注意不要以逗号为分隔符进行处理，csv数据格式以引号外的逗号为分隔符**_

**Spark**

建议下载数据后，将下载的压缩文件放于hdfs的以日期建立目录结构，同一小时或者同一天的数据放在同一目录下，然后通过spark streaming的fileStream接口监控根目录，读取变动的文件内容。

```text
streamingContext.fileStream[KeyClass, ValueClass, InputFormatClass](dataDirectory)
```

在依赖中添加：

```text
groupId: com.databricks
artifactId: spark-csv_2.10
version: 1.4.0
```

具体数据操作参考spark-csv\([https://github.com/databricks/spark-csv](https://github.com/databricks/spark-csv)\)

**Hive操作**

可以参考Hive对CSV数据操作的支持 [https://cwiki.apache.org/confluence/display/Hive/CSV+Serde](https://cwiki.apache.org/confluence/display/Hive/CSV+Serde)

目前暂未测试该方式～

```text
create external table xxx
```

**数据格式调整处理（以java为例）**

新建maven project，在pom.xml中添加以下依赖

```text
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-compress</artifactId>
      <version>1.12</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-csv -->
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-csv</artifactId>
      <version>1.4</version>
    </dependency>
```

而后在读取数据的方法中：

```text
    GzipCompressorInputStream stream = new GzipCompressorInputStream(new BufferedInputStream(new FileInputStream("data/test.gz")));

    Reader reader = new InputStreamReader(stream);
    Iterable<CSVRecord> records = CSVFormat.DEFAULT.parse(reader);
    for (CSVRecord record : records) {
        System.out.println(record);
    }
```

上例中，数据读取依赖于commons-compress与commons-csv库，同样在python中有类似的数据处理库。

