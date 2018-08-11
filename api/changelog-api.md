# API 更新日志

### 更新时间 2018年6月19日 版本号：V18.11.1

**新功能：**

1. 新增 collectWebViewUserAgent 接口，用户可以选择是否采集用户 UserAgent。
2. Configuration 删除 collectMacAddress接口。

### 更新时间 2018年5月22日 版本号：V18.8.1

**新功能：**

1. 新增[用户删除接口](delete-visitor-api.md)，满足 GDPR 的要求。

### 更新时间 2018年4月8日 版本号：V18.5.0

**新功能：**

1. [原始数据导出 2.0 API 功能](raw-data-export-new.md)。提供最新 SDK 2.x 的原始数据导出支持。
2. 用户变量分类扩展API。提供了服务器对接（S2S）的方式上传用户维度的API。

### 2017-02-21 增加自定义事件原始数据导出

1. visit数据增加新字段lat, lng, 为mobile平台采集的gps信息
2. 增加打点数据的导出，具体格式参考custom\_attr表字段说明

### 2016-09-08 增加访问级别的query与ps页面信息字段

1. 在visit级别增加新字段query，用于提取用户访问时的utm字段信息
2. 在page级别增加新字段pagegroup, ps1~ps10，对应sdk采集时设置的页面信息字段

### 2016-08-09 导出数据增加action\_tag与rules两张数据表

1. 导出数据中增加action\_tag数据，用于关联action级别数据中的action\_id与rule\_id字段
2. rules表用于关联rule\_id与rule名称，便于进一步分析GrowingIO平台圈选的标签数据
3. action\_tag与之前的导出数据接口一致，在获取的下载链接中多出action\_tag一项，而rules需要通过额外接口获取

### 2016-07-30 在action级别数据中增加字段index，info。

1. index用于标记列表中的第几项项目，为bigint类型
2. info用于用户自定义添加的标识信息，string类型，对应数据采集时growingAttributesInfo定义的属性
3. 此次修改仅在action级别增加字段信息，visit与page不受影响

### 2016-06-05

1. 认证的时候需要传入 tm 参数，tm 是当前时间戳（**毫秒**\)
2. insights API 支持传入 expire 参数，来控制链接过期时间，默认 5 分钟。
3. insights API 可以请求小时数据，比如 2016060510，表示获取6月5号10点到11点的数据。

