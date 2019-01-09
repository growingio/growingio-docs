# Java 埋点 SDK（Beta）

## 集成Java SDK

在服务端 Java 应用中集成 Java SDK，来上报离线的用户行为。

我们推荐使用 [Maven](http://search.maven.org/) 管理 Java 项目，请在 `pom.xml` 文件中，添加以下依赖信息，Maven 将自动获取Java SDK 并更新项目配置。

{% code-tabs %}
{% code-tabs-item title="pom.xml" %}
```markup

    <dependencies>
      // ...
      <dependency>
        <groupId>io.growing.sdk.java</groupId>
        <artifactId>growingio-java-sdk</artifactId>
        <version>1.0.1</version>
      </dependency>
    </dependencies>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

若出现依赖冲突的问题（例如运行时找不到类），可以使用 `standalone` 版本：

{% code-tabs %}
{% code-tabs-item title="pom.xml" %}
```markup

    <dependencies>
      // ...
      <dependency>
        <groupId>io.growing.sdk.java</groupId>
        <artifactId>growingio-java-sdk</artifactId>
        <version>1.0.1</version>
        <classifier>standalone</classifier>
      </dependency>
    </dependencies>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

目前，Java SDK 支持的Java版本为 Java 1.6+。

## 配置Java SDK

在Java SDK的jar包中，包含了一个默认的配置文件

{% code-tabs %}
{% code-tabs-item title="gio\_default.properties" %}
```text
#项目采集端地址
api.host=https://api.growingio.com

#项目ID
#project.id=填写您项目的AccountID

#消息发送间隔时间,单位ms（默认 100）
send.msg.interval=100

#消息发送线程数量
send.msg.thread=3

#消息队列大小
msg.store.queue.size=500

# 数据压缩 false:不压缩, true:压缩
# 不压缩可节省cpu，压缩可省带宽
compress=true

# 日志输出级别（debug | error）
logger.level=debug

# 自定义日志输出实现类
logger.implemention=io.growing.sdk.java.logger.GioLoggerImpl

# 运行模式，test：仅输出消息体，不发送消息，production：发送消息
run.mode=test
```
{% endcode-tabs-item %}
{% endcode-tabs %}

其中，开发者需要根据自己的情况修改配置参数，保存为gio.properties，并放置在自己Java程序的classpath之中。例如：

{% code-tabs %}
{% code-tabs-item title="gio.properties" %}
```text
#项目采集端地址
api.host=https://api.growingio.com

#项目ID
project.id=xxxxxxxxx

# 日志输出级别
logger.level=error

# 运行模式，test：仅输出消息体，不发送消息，production：发送消息
run.mode=production
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Java SDK会优先读取gio.properties中的配置

## 调用SDK API发送事件

{% code-tabs %}
{% code-tabs-item title="SDK-Demo.java" %}
```text
//事件行为消息体
GIOEventMessage eventMessage = new GIOEventMessage.Builder()
    .eventTime(System.currentTimeMillis())            // 事件事件，默认为系统时间（选填）
    .eventKey("BuyProduct")                           // 事件标识 (必填)
    .loginUserId("417abcabcabcbac")                   // 登录用户ID (必填)
    .addEventVariable("product_name", "苹果")          // 事件级变量 (选填)
    .addEventVariable("product_classify", "水果")      // 事件级变量 (选填)
    .addEventVariable("product_price", 14)            // 事件级变量 (选填)
    .build();

//上传事件行为消息到服务器
GrowingAPI.send(eventMessage);

```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 程序调试

GrowingIO建议您按照如下步骤进行埋点数据的开发联调

1. 在GrowingIO的网站中创建自定义事件以及对应的自定义事件变量
2. 在您的Java项目中的pom.xml中增加GrowingIO Java SDK的依赖（首次集成需要）
3. 参考上面的文档编写gio.properties文件并将run.mode定义为test
4. 在您的Java项目中找到合适的埋点位置，参考上面的例子填入自定义事件需要的字段
5. 执行对应修改部分的单元测试，或者编写一段测试程序运行修改部分的代码，确保触发埋点事件
6. 在输出的日志中查找是否包含期望事件内容
7. 在以上步骤全部成功后，修改gio.properties文件并将run.mode定义为production



