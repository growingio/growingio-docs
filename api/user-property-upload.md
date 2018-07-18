# 用户变量上传 API

* [1. 接口](user-property-upload.md#1-jie-kou)
* [2. 认证](user-property-upload.md#2-ren-zheng)
* [3. 旧版本上传接口](user-property-upload.md#3-jiu-ban-ben-shang-chuan-jie-kou)

GrowingIO 支持通过离线的方式批量上传登录用户变量，配合 SDK 中上传的登录用户 id，可以在不发版的情况下更新用户变量规则。

### 1. 接口

{% api-method method="post" host="https://data.growingio.com/:ai/loginUserId" path="" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="ai" type="string" required=true %}
项目 id
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="auth" type="string" required=true %}
针对每条数据独立生成的认证，计算方式见本文档第二节
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="userProperty1" type="string" required=false %}
在 GrowingIO 系统内定义的用户属性 （如 gender\)
{% endapi-method-parameter %}

{% api-method-parameter name="userProperty2" type="string" required=false %}
在 GrowingIO 系统内定义的用户属性（如 user\_name\)
{% endapi-method-parameter %}

{% api-method-parameter name="loginUserId" type="string" required=true %}
登录用户 id
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

Body 内的 userProperty 1-N 为您在 GrowingIO 系统内定义的用户属性的 key，如 user\_name, gender 等。支持使用数组的方式一次上传多条数据，一次性最多上传 100 条，body 大小最大限制为 2M。

一次上传一条：

```text
{
    "loginUserId":"1234",
    "user_name":"张三",
    "gender":"男"
}
```

一次上传多条：

```text
[
{
    "loginUserId":"1234",
    "user_name":"张三",
    "gender":"男"
},
{
    "loginUserId":"1235",
    "user_name":"李四",
    "gender":"女"
}
]
```

### 2. 认证

为防止误传和恶意攻击， GrowingIO 服务器会对收到的每条数据做校验，因此需要在 query 参数中提供校验码。校验码生成代码见下方示例，其中 keyArray 为 loginUserId，一次性上传多条时，使用逗号隔开，如上方示例中，第一条 keyArray 为 `1234`，第二条为 `1234,1235`。

Java:

```java
public String authToken(String projectKeyId, String secretKey, String keyArray) throws Exception {
    String message = "ai="+projectKeyId+"&loginUserId="+keyArray;
    Mac hmac = Mac.getInstance("HmacSHA256");
    hmac.init(new SecretKeySpec(secretKey.getBytes("UTF-8"), "HmacSHA256"));
    byte[] signature = hmac.doFinal(message.getBytes("UTF-8"));
    return Hex.encodeHexString(signature);
}
```

Scala:

```scala
def authToken(projectKeyId: String, secretKey: String, keyArray: String): String = {
  val message = s"ai=$projectKeyId&loginUserId=$keyArray"
  val hmac: Mac = Mac.getInstance("HmacSHA256")
  hmac.init(new SecretKeySpec(secretKey.getBytes("UTF-8"), "HmacSHA256"))
  val signature = hmac.doFinal(message.getBytes("UTF-8"))
  Hex.encodeHexString(signature)
}
```

Python:

```python
#coding:utf-8 
import hashlib
import hmac

def authToken(projectKeyId,secretKey,keyArray):
    message = ("ai=" + projectKeyId + "&loginUserId=" + keyArray).encode('utf-8')
    signature = hmac.new(bytes(secretKey.encode('utf-8')), bytes(message), digestmod=hashlib.sha256).hexdigest()
    return signature
```

PHP:

```text
function authToken($projectKeyId, $secretKey, $keyArray)
{
   $message="ai=".$projectKeyId."&loginUserId=".$keyArray;
   return hash_hmac('sha256',$message, $secretKey, false);
}
```



### 3. 旧版本上传接口

之前使用旧版本上传接口的请尽快升级到新版本接口，避免数据上传结果和线上配置出现不一致。

{% api-method method="post" host="https://data.growingio.com/saas/:ai/user" path="" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="ai" type="string" required=true %}
项目 id
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="auth" type="string" required=true %}
针对每条数据独立生成的认证
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="cs2" type="string" required=true %}
用户属性2
{% endapi-method-parameter %}

{% api-method-parameter name="cs1" type="string" required=true %}
登录用户 id
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

旧版本上传接口使用 cs1-cs20 的方式上传用户属性，对应的签名计算中 keyArray 为 user\_id:1234,1235。

