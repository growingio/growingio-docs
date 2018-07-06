# 原始数据导出 服务端SDK

**目前仅提供 Java 版本的 GrowingIO 数据平台 API SDK**

### 使用方式 {#使用方式}

#### 在maven中添加依赖 {#在maven中添加依赖}

```text
<repositories>
  <repository>
  <id>growingio</id>
  <url>https://oss.sonatype.org/content/repositories/snapshots</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>com.growingio.growingapi</groupId>
    <artifactId>growing-api-java</artifactId>
    <version>0.0.3-SNAPSHOT</version>
  </dependency>
</dependencies>
```

或者将assembly包添加到classpath的lib路径中。`git clone`代码之后直接运行`mvn clean package`获取assembly jar包。

然后添加配置文件\(可以在resources目录下直接添加\),若是使用GrowingDownloadApi,则添加growingApi.conf。也可以自己实现对应的DownloadApi,override其中的store方法即可。

### AppDemo.java {#appdemojava}

#### growingApi.conf配置 {#growingapiconf配置}

```text
app {
# configure ai value, 在项目管理页面内（管理员权限）
# ai: "abc12345678caf"

# configure project Id, 此字段可在项目页面url中找到。
# 如："https://www.growingio.com/admin/projects/nxog09md/dashboard"中的"nxog09md"
# projectId: "nxoa08md"

# 秘钥,在项目管理页面内（管理员权限）
# secretKey: "xxx"

# 公钥,在项目管理页面内（管理员权限）
# publicKey: "xxx"


# 下面两个配置项为GrowingDownloadApi内配置项

# 存储文件的绝对路径地址,可以override store()方法,控制数据存储
# store: "/tmp"

# 选择是否解压存储在目录下
# uncompress: true
}
```

#### 调用方式 {#调用方式}

```text
  GrowingDownloadApi api = new GrowingDownloadApi();
  api.download("2016071221"); //此函数可选第二个参数 expire，设定超时时间
```

