# 数据管理 API \(GDPR\)

为符合 GDPR 规范，GrowingIO 提供删除用户原始数据的功能。

### 删除用户API

从原始数据中删除用户的数据，支持批量删除。

{% api-method method="post" host="https://data.growingio.com/:projectId/deleteVisitor?auth=:auth\_token" path="" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="projectId" type="string" required=true %}
 项目ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=false %}
application/json
{% endapi-method-parameter %}

{% api-method-parameter name="Access-Token" type="string" required=true %}
public key，GrowingIO分配的公钥
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="auth" type="string" required=true %}
auth token
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="visitUserId" type="array" required=true %}
user id
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

Body由多个visitUserId组成，一次性最多上传 100 条，大小最大限制为 2M。示例如下：

```javascript
{
  "visitUserId":["abcdef","bcdefg",...]
}
```

#### 认证

为防止误传和恶意攻击， GrowingIO 服务器会对收到的每条数据做校验，因此需要在 query 参数中提供校验码 auth。校验码生成代码见下方示例，其中 keyArray 为 visitUserId，一次性上传多条时，使用逗号隔开，如上方示例中，keyArray为`abcdef,bcdefg` 。

> Java

```java
public String authToken(String projectKeyId, String secretKey, String keyArray) throws Exception {
    String message = "projectId="+projectKeyId+"&visitUserId="+keyArray;
    Mac hmac = Mac.getInstance("HmacSHA256");
    hmac.init(new SecretKeySpec(secretKey.getBytes("UTF-8"), "HmacSHA256"));
    byte[] signature = hmac.doFinal(message.getBytes("UTF-8"));
    return Hex.encodeHexString(signature);
}
```

> 其他语言可以参考[用户变量上传 API](user-property-upload.md#2-ren-zheng)

#### 特别说明

* user id 是 GrowingIO SDK 生成的 cookie，您可以从导出的原始数据中获得。
* 删除用户数据后，该部分用户的细节数据\(包括用户细查数据、以该部分用户ID做数据查询\) 都不会保留在GrowingIO平台；但汇总数据（比如 "访问用户量" ）不会受影响。
* 汇总数据不受影响的原因：GrowingIO收到该用户数据后启动汇总数据计算，这个过程是不可逆的。删除用户数据请求是在用户数据发送至GrowingIO之后的操作，该请求不会影响前置的汇总数据计算。请您知晓，汇总数据不受影响跟GDPR的要求没有任何相悖。



