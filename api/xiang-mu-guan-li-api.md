---
description: 提供项目配置和管理
---

# 项目管理 API

### 1. 用户管理 API

{% api-method method="post" host="https://www.growingio.com" path="/api/v1/projects/:project\_id/accounts/delete" %}
{% api-method-summary %}
从项目内移除用户
{% endapi-method-summary %}

{% api-method-description %}
 通过邮箱删除用户，用于清理项目内失效用户
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="email" type="string" required=true %}
需要删除的用户邮箱
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{    "status": "success"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
 请求失败，message 中会有错误消息
{% endapi-method-response-example-description %}

```
{    "status": "fail",    "message": "error message"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
 认证失败
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



### 2. 打点事件管理 API

通过接口获取打点事件信息，创建打点事件。

{% api-method method="get" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/dim/events" %}
{% api-method-summary %}
获取打点事件列表
{% endapi-method-summary %}

{% api-method-description %}
返回当前项目打点事件，没有结果时返回空数组。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[    {        "id": "id1", // 事件 uid        "key": "test1", // 事件标识符        "name": "测试1", // 事件名称        "description": "测试", // 事件描述        "type": "counter", // 事件类型, counter: 计数器, number: 数值        "attrs": [ // 事件级变量            {                "id": "id2", // 变量 uid                "key": "labore111", // 变量标识符                "name": "变量1", // 变量名称                "type": "String", // 变量类型, 目前有 String, Int, Double 类型                "projectId": "id3", // 项目 uid                "createdAt": 1534820494000, // 变量创建时间，unix 毫秒时间戳                "updatedAt": 1534820494000 // 变量最后一次更新时间, unix 毫秒时间戳            }        ],        "projectId": "id3", // 项目 uid        "createdAt": 1534426936000, // 事件创建时间, unix 毫秒时间戳        "updatedAt": 1535425076000, // 事件最后一次更新时间, unix 毫秒时间戳        "creator": "User1", // 创建人        "updater": "User1", // 最后一次更新人        "eventType": "dash" // 事件类型, 目前仅有 dash，表示为打点事件    }]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/dim/events" %}
{% api-method-summary %}
创建打点事件
{% endapi-method-summary %}

{% api-method-description %}
创建打点事件，注意目前仅支持创建，不支持修改，如需修改，请到 GrowingIO 平台管理界面修改。打点事件支持批量创建，如果一次仅创建一条，boyd 内也需要使用数组形式。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="attrs" type="array" required=false %}
事件关联的维度属性
{% endapi-method-parameter %}

{% api-method-parameter name="type" type="string" required=true %}
事件类型， counter: 计数器，number: 数值
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
事件描述
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
事件名称，最大长度为 30
{% endapi-method-parameter %}

{% api-method-parameter name="key" type="string" required=true %}
事件标识符，只允许「字母或下划线开头，包含下划线，字母，数字的字符串」，最大长度为 50
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

attrs 字段为事件需要关联的维度属性，以下为 attrs 对应的字段：

| 字段 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | String | 事件级变量 uid |
| key | String | 事件级变量标识符 |
| name | String | 事件级变量类型，目前有 String, Int, Double 三种类型 |

{% api-method method="get" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/events" %}
{% api-method-summary %}
获取事件级变量
{% endapi-method-summary %}

{% api-method-description %}
获取项目下打点维度列表
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[    {        "id": "id1", // 打点维度 uid        "key": "test1", // 打点维度标识符        "name": "测试", // 打点维度名称        "type": "String", // 打点维度类型，目前有 String, Int, Double 三种类型        "projectId": "pid1", // 项目 uid        "createdAt": 1534820494000, // 打点维度创建时间， unix 毫秒时间戳        "updatedAt": 1534820494000 // 打点维度最后更新时间， unix 毫秒时间戳    }]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/events" %}
{% api-method-summary %}
创建事件级变量
{% endapi-method-summary %}

{% api-method-description %}
创建事件级变量，目前仅支持创建，不支持修改，如需修改，请到 GrowingIO 平台管理界面修改。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="description" type="string" required=false %}
变量描述，最大长度 150
{% endapi-method-parameter %}

{% api-method-parameter name="type" type="string" required=false %}
变量类型，只能为 String, Int, Double 中的一种，默认为 String
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
变量名称，最大长度为 30
{% endapi-method-parameter %}

{% api-method-parameter name="key" type="string" required=true %}
变量标识符，只允许「字母或下划线开头，包含下划线，字母，数字的字符串」，最大长度为 50
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/pages" %}
{% api-method-summary %}
获取页面级变量
{% endapi-method-summary %}

{% api-method-description %}
返回当前项目的页面级变量列表
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[  {        "id": "id1", // 变量 uid        "key": "test1", // 变量标识符        "name": "测试", // 变量名称        "mode": "DIRECT", // 变量创建方式，DIRECT: 通过接口或则打点管理创建， PAGE: 通过页面圈选创建        "projectId": "pid1", // 项目 uid        "createdAt": 1521446011241, // 创建时间， unix 毫秒时间戳        "updatedAt": 1534163546862, // 最后一次更新时间， unix 毫秒时间戳        "mappingId": 15 // 变量序号    }]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/pages" %}
{% api-method-summary %}
创建页面级变量
{% endapi-method-summary %}

{% api-method-description %}
创建页面级变量，目前仅支持创建，不支持修改，如需修改，请到 GrowingIO 平台管理界面修改。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="description" type="string" required=false %}
变量描述，最大长度 150
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
变量名称，最大长度 30
{% endapi-method-parameter %}

{% api-method-parameter name="key" type="string" required=true %}
变量标识符，字母或下划线开头，包含下划线，字母，数字的字符串」，最大长度为 50
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/peoples" %}
{% api-method-summary %}
获取登录用户变量
{% endapi-method-summary %}

{% api-method-description %}
获得当前项目登录用户变量列表
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[   {        "id": "id1", // 变量 uid        "key": "test1", // 变量标识符        "name": "测试", // 变量名称        "description": "测试", // 变量描述        "projectId": "pid1", // 项目 uid        "createdAt": 1532416276546, // 变量创建时间， unix 毫秒时间戳        "updatedAt": 1532416276546, // 变量最后一次修改时间， unix 毫秒时间戳        "attribution": "mostRecent" // 变量类型，mostRecent: 最近归因，final: 最终归因    }]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/peoples" %}
{% api-method-summary %}
创建登录用户变量
{% endapi-method-summary %}

{% api-method-description %}
创建登录用户变量，目前仅支持创建，不支持修改，如需修改请到 GrowingIO 平台管理界面修改。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="attribution" type="string" required=true %}
归因方式，mostRecent: 最近归因，final: 最终归因
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
变量描述，最大长度为 150
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
变量名称，最大长度为 30
{% endapi-method-parameter %}

{% api-method-parameter name="key" type="string" required=true %}
变量标识符，只允许「字母或下划线开头，包含下划线，字母，数字的字符串」，最大长度为 50
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://www.growingio.com" path="/v1/api/projects/:project\_id/vars/quotas" %}
{% api-method-summary %}
获取打点数量限额
{% endapi-method-summary %}

{% api-method-description %}
GrowingIO 根据付费版本设定不同的打点事件和维度的限额，通过此接口获取您项目的限额。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Project UID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
认证 Token
{% endapi-method-parameter %}

{% api-method-parameter name="X-Client-Id" type="string" required=true %}
GrowingIO 分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{    "event": 50000, // 打点事件限额    "var": 100, // 事件级变量限额    "pvar": 50, // 页面级变量限额    "ppl": 50000 // 登录用户变量限额}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

