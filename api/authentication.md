# GrowingIO API 认证

* [1.术语](authentication.md#1-shu-yu)
* [2.认证](authentication.md#认证)
* [3.API 定义](authentication.md#api-定义)

### 1.术语

* **公钥（** **X-Client-Id）**: GrowingIO 分配的项目公钥，请求时用来做身份校验的一串字符码
* **私钥**: 双方所约定的加密算法的私钥
* **ai**: 项目ID，可在项目管理的项目概览里获得这串ID，也是集成 SDK 时 setAccountId 所用的部分。
* **project**: 项目UID，访问项目的时候，页面 URL 以 /projects/:project\_uid 开头，例如 "[https://www.growingio.com/admin/projects/nxog09md/dashboard"中的"nxog09md"。](https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82%29]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82]%28https://www.growingio.com/admin/projects/nxog09md/dashboard%22%E4%B8%AD%E7%9A%84%22nxog09md%22%E3%80%82%29%29/)
* **auth**: 通过认证算法计算出来的签名，见第二部分示例代码
* **tm**: 当前请求时间戳

### 2.认证 {#认证}

```text
____________                  ___________    (ai/project/auth)   _____________
|          |                 |           |--(B) Authorization ->|             |
|          |--(A) Request  ->|   Client  |                      |             |
|  Client  |<-(D)-- Code  ---|   Server  |<--(C) Auth Grant  ---|  GrowingIO  |
|          |                 |___________|     (Auth Code)      |   Server    |
|          |                                                    |             |
|          |--(E)------------  Access Code  ------------------> |             |
|__________|                                                    |_____________|
```

GrowingIO 会给每个项目分配个公钥\(X-Client-Id\)和私钥。具体认证步骤如下。

1. 用户打开客户页面，会向 Client Server 发起一个请求
2. Client Server 在渲染页面时，会向 Growing Server 做认证请求，请求参数包括 ai, project 和 auth，头部参数包含 公钥（X-Client-Id ）。

   ```text
   POST https://www.growingio.com/auth/token -H "X-Client-Id: client-id" -d "project=123abc&ai=13411891aaffda&tm=1465020309123&auth=ab3i5dazoo58314l0qqrj1aslfj1ldfaqeroqi"
   ```

   其中，auth 的计算方式是，

   Java 版本示例代码

   ```text
   public String authToken(String secret, String project, String ai, Long tm) throws Exception {
     String message = "POST\n/auth/token\nproject="+project+"&ai="+ai+"&tm="+tm;
     Mac hmac = Mac.getInstance("HmacSHA256");
     hmac.init(new SecretKeySpec(secret.getBytes("UTF-8"), "HmacSHA256"));
     byte[] signature = hmac.doFinal(message.getBytes("UTF-8"));
     return Hex.encodeHexString(signature);
   }

   authToken("这里是 GrowingIO 给项目分配的私钥", "项目UID", "项目ID", "申请时间戳")
   ```

   Scala 版本示例代码

   ```text
   import javax.crypto.Mac
   import javax.crypto.spec.SecretKeySpec
   import org.apache.commons.codec.binary.Hex

   def authToken(secret: String, project: String, ai: String, tm: Long) = {
    val messages = s"POST\n/auth/token\nproject=$project&ai=$ai&tm=$tm"
    val hmac = Mac.getInstance("HmacSHA256")
    hmac.init(new SecretKeySpec(secret.getBytes("UTF-8"), "HmacSHA256"))
    val signature = hmac.doFinal(messages.getBytes("UTF-8"))
    Hex.encodeHexString(signature)
   }

   authToken("这里是 GrowingIO 给项目分配的私钥", "项目UID", "项目ID", "申请时间戳")
   ```

   PHP版本示例代码

   ```text
   <?php

   function authToken($secret, $project, $ai, $tm) {
       $str = "POST\n/auth/token\nproject=${project}&ai=${ai}&tm=${tm}";
       $signature = hash_hmac("sha256", $str, $secret);
       return $signature;
   }

   authToken("这里是 GrowingIO 给项目分配的私钥", "项目UID", "项目ID", "申请时间戳")
   ?>
   ```

   Python 版本示例代码

   ```text
   import hashlib
   import hmac

   def authToken(secret, project, ai, tm):
     message = ("POST\n/auth/token\nproject=" + project + "&ai=" + ai + "&tm=" + tm).encode('utf-8')
     signature = hmac.new(bytes(secret.encode('utf-8')), bytes(message), digestmod=hashlib.sha256).hexdigest()
     return signature

   authToken("这里是 GrowingIO 给项目分配的私钥", "项目UID", "项目ID", "申请时间戳")
   ```

3. Growing Server 收到数据后，会用请求的 body 和 key 做同样的加密，计算是不是匹配。如果匹配，返回认证码给 Client Server。
4. Client Server 拿到认证码后，可以使用这个认证码去请求在 GrowingIO 中的数据，比如看板和单图。

### 3.API 定义 {#api-定义}

#### Resource {#resource}

POST [https://www.growingio.com/auth/token](https://www.growingio.com/auth/token)

#### Authorization {#authorization}

在 Header 里面添加一个属性：

| 名字 | 类型 | 描述 | 示例 |
| --- | --- |
| X-Client-Id | String | GrowingIO 分配的公钥，请在GrowingIO后台“项目配置”页面获取 | X-Client-Id: 123abc |

#### Query Parameter {#query-parameter}

| 名字 | 类型 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| ai | String | 项目ID | 2a1b4018cd954ec2bcc69da5138bdb96 |
| project | String | 项目UID | 123abc |
| tm | Long | 申请时间戳 | 1465020309123 |
| auth | String | 加密签名 | ab3i5dazoo58314l0qqrj1aslfj1ldfaqeroqi |

#### Response {#response}

```text
   {
     "status":"success",
     "code":"2RhY0XZ9xyBfayAPm0aa5CoJhDJkEUcmRiBJBT6XyeIXhHrdz334Tf3I85Esm74Q"
   }
```

