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
{
    "status": "success"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
 请求失败，message 中会有错误消息
{% endapi-method-response-example-description %}

```
{
    "status": "fail",
    "message": "error message"
}
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



