# 埋点事件与变量

## cstm——埋点事件

| 原始数据导出 2.0 字段名称 | 原始数据导出 1.0 字段名称 | 字段格式 | 字段说明 | 示例值 | 备注 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| visitUserId | \_userId（新旧不同） | string\(64\) | 访问用户ID（visituser id） | fc55728b-41ab-42ff-8b1f-714e44c65fd6 | 访问用户 ID 唯一标识 |
| sessionId | \_sessionId（新旧不同） | string\(50\) | 访问ID（session id） | 6b5099c7-6006-422d-92ac-4f3bf4ddd37c | 当前访问唯一标识 |
| time | \_eventTime（新旧不同） | bigint | 时间戳（time） | 1506069592985 | 请求在用户端发生的时间戳 |
| sendTime | （新加） | bigint | 发送时间（send time） | 1507897826891 | 请求在SDK发送的时间戳 |
| pageTime | （新加） | bigint | 页面时间（page time） | 1516349263375 | pvar对应的页面请求的产生时间 |
| domain | （新加） | string\(100\) | 域名（domain） | www.growingio.com | 访问的域名，当为 iOS / Android 时，为 app 包名 |
| page | path（新旧不同） | string\(1024\) | 页面（page） | pages/index | 用户访问的当前页面 |
| queryParameters | （新加） | string\(1024\) | 查询参数（query arameters） | [http://www](http://www/).[growingio.com?cid=1234567](http://growingio.com/?cid=1234567) | 当前网站页面URL中的查询参数 |
| eventName | （新加） | string\(50\) | 事件名称（event name） | revenue | 自定义事件的标识符 |
| eventNumber | （新加） | double | 事件数值（event number） | 99.99 | 自定义事件的值 |
| eventVariable | （新加） | map&lt;string, string&gt; | 事件级变量（event variable） | {"price": "50.0", "item": "101"} | 自定义事件级变量 |
| loginUserId | \_cs1（新旧不同） | string\(200\) | 登录用户ID（customer attributes 1） | user12345 | 登录用户ID，推荐使用那些不能定位到个人的ID信息，通常为企业内部使用的CRM ID |
| pageRequestId | （新加） | string\(23\) | GrowingIO系统页面请求内部ID | 15208995970115f7e2c153f | 该自定义事件所归属的 page 事件 id |

## pvar——自定义页面变量

| 原始数据导出 2.0 字段名称 | 字段格式 | 字段说明 | 示例值 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| visitUserId | string\(64\) | 访问用户ID（visit user id） | 1ba42333-87f2-3cc4-bb42-ac4176526796 | 访问用户 ID 唯一标识 |
| sessionId | string\(50\) | 访问ID（session id） | c6575ef5-5c06-443e-bf6e-b12e1e37a3f8 | 当前访问唯一标识 |
| time | bigint | 时间戳（time） | 1520899220665 | 请求在用户端发生的时间戳 |
| sendTime | bigint | 发送时间（send time） | 1520899221211 | 请求在SDK发送的时间戳 |
| pageTime | bigint | 页面时间（page time） | 1520899221209 | pvar对应的页面请求的产生时间 |
| domain | string\(100\) | 域名（domain） | www.growingio.com | 访问的域名，当为 iOS / Android 时，为 app 包名 |
| page | string\(1024\) | 页面（page） | /funnel | 用户访问的当前页面 |
| pageVariable | map&lt;string, string&gt; | 页面级变量（page variable） | {"category": "funnel"} | 页面级变量键值对 |
| pageRequestId | string\(23\) | GrowingIO系统页面请求内部ID | 15208995970115f7e2c153f | 该页面级变量所归属的 page 事件 id |

## evar——转化变量

| 原始数据导出 2.0 字段名称 | 字段格式 | 字段说明 | 示例值 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| visitUserId | string\(64\) | 访问用户ID（visit user id） | c6dc7078-19a8-43c8-a728-d7f78f38bc7b | 访问用户 ID 唯一标识 |
| sessionId | string\(50\) | 访问ID（session id） | 372f87d0-c743-4b6b-a4c3-3833b90ce5e2 | 当前访问唯一标识 |
| time | bigint | 时间戳（time） | 1521331185777 | 请求在用户端发生的时间戳 |
| sendTime | bigint | 发送时间（send time） | 1521331204282 | 请求在SDK发送的时间戳 |
| domain | string\(100\) | 域名（domain） | www.growingio.com | 访问的域名，当为 iOS / Android 时，为 app 包名 |
| conversionVariable | map&lt;string, string&gt; | 转化变量（conversion variable） | {"keyword":"retention"} |  |

