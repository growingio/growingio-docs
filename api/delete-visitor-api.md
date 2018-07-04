# 删除用户 API

1. 调用DeleteVisitor API之前先准备好哪些访问用户ID的数据需要删除
2. 基于准备好需要删除的访问用户ID，调用DeleteVisitor API
   * **服务器请求接口**
     1. 服务器请求URL：[https://data.growingio.com/{projectId}/deleteVisitor?auth={auth\_token}](https://data.growingio.com/%7BprojectId%7D/deleteVisitor?auth={auth_token})
     2. 服务器请求header：
        * Access-Token = publicKey
        * Content-Type = application/json
     3. 服务器请求body： { "visitUserId":\["abcdef","bcdefg",...\] }
   * **示例说明**：
     1. visitUserId是GrowingIO生成的ID，用以代表匿名的访问用户
     2. deleteVisitor API，单个请求上传的数据包总大小不超过2MB，内含u的个数不超过100个
   * **{auth\_token}由如下三个参数计算生成**
     1. projectId: 由GrowingIO分配给您的项目ID
     2. secretKey：由GrowingIO分配给您的私钥
     3. keyArray：取visitUserId数组中的值，并用逗号进行拼接。如：abcdef,bcdefg
   * **计算公式**
     1. 以secretKey为key
     2. 使用SHA256算法计算字符串"projectId=${projectId}&visitUserId=${keyArray}"的hash值

### 特别说明 {#特别说明}

1. 删除用户数据后，该部分用户的细节数据\(包括用户细查数据、以该部分用户ID做数据查询\) 都不会保留在GrowingIO平台；但汇总数据（比如 "访问用户量" ）不会受影响。
2. 汇总数据不受影响的原因：GrowingIO收到该用户数据后启动汇总数据计算，这个过程是不可逆的。删除用户数据请求是在用户数据发送至GrowingIO之后的操作，该请求不会影响前置的汇总数据计算。请您知晓，汇总数据不受影响跟GDPR的要求没有任何相悖。

