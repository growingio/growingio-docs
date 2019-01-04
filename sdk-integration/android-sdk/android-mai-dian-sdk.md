---
description: >-
  埋点 SDK 的目标用户是使用第三方插件开发的 APP， 比如使用 Weex、APICloud
  等。在这些平台中，我们无法自动采集用户的点击事件和页面浏览事件等，需要依赖调用自定义事件和变量 API来进行数据采集。 如果您的 APP 使用
  Android 原生开发，并且希望自动采集用户的点击事件、页面浏览事件等无埋点事件， 请集成 Android无埋点SDK 。
---

# Android 埋点 SDK

## 集成埋点 SDK

### 1.添加依赖

Gradle编译环境（AndroidStudio）

**在module级别的build.gradle文件中添加`vds-android-agent`依赖、项目ID和 URL Scheme**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种：

* 新产品的 URL Scheme ：登录官网 -&gt;点击“设置”icon -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt;  URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
* 现有产品的 URL Scheme ：登录官网 -&gt; 点击“设置” icon -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![&#x5E94;&#x7528;&#x7BA1;&#x7406;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2857%29.png)

您的项目ID获取方式是：点击“设置”icon-&gt;点击“项目配置”-&gt;即可看到您的项目ID

```groovy
android {
    defaultConfig {
        resValue("string", "growingio_project_id", "您的项目ID")
        resValue("string", "growingio_url_scheme", "您的URL Scheme")
    }
}
dependencies {
    //GrowingIO 埋点 SDK
    implementation 'com.growingio.android:vds-android-agent:track-2.6.7@aar'
}
```

### 2.添加URLScheme和权限

把URL Scheme添加到您的项目，以便在使用 [Mobile Debugger](../growingio-debugger/) 的时候通过 Deep Link 方式唤醒您的程序。将该产品的 URLScheme 添加到你的 AndroidManifest.xml 中的 LAUNCHER Activity下。例如：

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.growingio.testdemo">

    <uses-permission android:name="android.permission.INTERNET" />
    <!--非危险权限，不需要运行时请求，Manifest文件中添加即可-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

    <!--请注意<application/>标签中的name属性值（这里为android:name=".MyApplication"）必须为您的Application类-->
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:name=".MyApplication"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            <!--请添加这里的整个 intent-filter 区块，并确保其中只有一个 data 字段-->
            <intent-filter>
                <data android:scheme="growing.您的URL Scheme" />
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
            </intent-filter>
            <!--请添加这里的整个 intent-filter 区块，并确保其中只有一个 data 字段-->
        </activity>
    </application>

</manifest>
```

### 3.初始化SDK

请将以下 GrowingIO.startWithConfiguration加在您的Application 的 onCreate 方法中

```java
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        GrowingIO.startWithConfiguration(this, new Configuration()     
            .setChannel("XXX应用商店")
        );
    }
}
```

（1）请确保将代码添加在`Application`的`onCreate`方法中，添加到其他方法中可能导致数据不准确。

（2）其中`GrowingIO.startWithConfiguration`第一个参数为`Application`对象。

（3）`setChannel`方法的参数定义了“自定义App渠道”这个维度的值。

### 4. 代码混淆

如果你启用了混淆，请在你的proguard-rules.pro中加入如下代码：

```text
-keep class com.growingio.** {
    *;
}
-dontwarn com.growingio.**
​
-keepnames class * extends android.view.View
-keepnames class * extends android.app.Fragment
-keepnames class * extends android.support.v4.app.Fragment
-keepnames class * extends androidx.fragment.app.Fragment
​
-keep class android.support.v4.view.ViewPager
-keep class android.support.v4.view.ViewPager$**{
  *;
}
-keep class androidx.viewpager.widget.ViewPager
-keep class androidx.viewpager.widget.ViewPager$**{
  *;
}
```

如果您使用了AndResGuard,请在白名单里添加GrowingIO,如下：

```text
R.string.growingio*
```

**添加代码之后，请先Clean项目，然后再进行编译，并在你的 Android App 安装了 SDK 后重新启动几次 App，保证行为采集数据自动发送给 GrowingIO，以便顺利完成检测。**

\*\*\*\*

## 重要配置

### **设置 Debug 模式**

#### **setDebugMode**

查看数据采集发送日志，能够在`Android Studio`中通过`Logcat`查看`GrowingIO`打印的数据发送日志，在 `APP` 的 `Application onCreate` 初始化`SDK`地方添加配置。

```java
setDebugMode(boolean debugMode);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| debugMode | boolean |  是 | false | 开启`GrowingIO`日志，true开启 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this,new Configuration()
    //BuildConfig.DEBUG 这样配置就不会上线忘记关闭
    .setDebugMode(BuildConfig.DEBUG)
    ...
    );
```



### 设置 Test 模式

#### setTestMode

实时发送数据，开启则不遵循移动网络状态下数据发送大小限制以及采集数据缓存30秒发送策略。为了方便开发者查看日志，一般和`setDebugMode`一起使用。在 `APP` 的 `Application onCreate` 初始化`SDK`地方添加配置。

```java
setTestMode(boolean testMode);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| testMode | boolean |  是 | false | 开启测试模式，true开启 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this,new Configuration()
    //BuildConfig.DEBUG 这样配置就不会上线忘记关闭
    .setTestMode(BuildConfig.DEBUG)
    ...
    );
```

\*\*\*\*

### 采集 GPS 数据

#### setGeoLocation

精确采集`GPS`数据，请在获取坐标后，调用接口设置位置信息。当用户下一次切换页面，或者发生点击行为时，`GPS`数据会被发送回`GrowingIO`。如果您不调用此接口也可以，我们会根据用户的`ip`定义用户位置，能够在最终的数据分析时看到`APP`用户地域分布。

```java
GrowingIO.getInstance().setGeoLocation(double latitude,double longitude);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| latitude | double |  是 |   无 | 纬度 |
| longitude | double |  是 |   无 | 经度 |

**示例代码：**

```java
//北京的经纬度
GrowingIO.getInstance().setGeoLocation(39.9046900000,116.4071700000);
```

{% hint style="warning" %}
对应的清除地理位置方法为 `clearGeoLocation();` 
{% endhint %}

### \*\*\*\*

### 多进程支持

#### setMutiprocess 

多进程支持，`SDK`默认不支持多进程使用， 但是可以通过`confiuration`进行设置支持多进程。 在`Application onCreate` 中初始化SDK代码块中配置。

```java
//支持多进程数据采集
setMutiprocess(boolean setMutiprocess);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| isMultiprocess | boolean |   是 |  false | 开启多进程数据采集 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                  .setMutiprocess(true)
                  );
```

{% hint style="info" %}
1. 为什么不默认支持多进程？

        跨进程通信是一个相对较慢的过程， 默认不开启， 可以满足大部分用户的要求。

   2. 哪些进程需要初始化SDK？

        需要使用SDK功能的进程需要初始化SDK， 所有的UI进程 + 部分Service进程\(如果这些进程中涉及手动打点\)。
{% endhint %}



### GDPR 数据采集开关

| 接口名称 | 含义 |
| :--- | :--- |
| disableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户不授权，不采集用户数据 |
| enableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户授权，采集用户数据 |



### Deep Link 回调参数获取

#### setDeeplinkCallback  

通过获取回调参数，GrowingIO SDK将会传递指定活动页面参数至您的App，请根据此参数和业务场景，执行相关的交互。

在 GrowingIO SDK 代码初始化部分配置。

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                .setDeeplinkCallback(new DeeplinkCallback() {
                            @Override
                            public void onReceive(Map<String, String> params, int status) {
        
                            }
                        })
            );
```

**返回值说明：**

| 返回值名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| params                | Map&lt;String,String&gt; | 自定义参数，您自定义的键值对 |
| status | int | DeeplinkCallback.SUCCESS ：自定义参数获取成功； DeeplinkCallback.PARSE\_ERROR ：解析异常；DeeplinkCallback.ILLEGAL\_URI ：非法URI； DeeplinkCallback.NO\_QUERY : 自定义参数为空。 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this, new Configuration()
    .setDeeplinkCallback(new DeeplinkCallback() {
                @Override
                public void onReceive(Map<String, String> params, int status) {
                     if (status == DeeplinkCallback.SUCCESS) {
                        //获得您的自定义参数，处理您的相关逻辑
                        Log.d("TestApplication", "DeepLink 参数获取成功，params" + params.toString());
                    }
                }
            })
);
```



## 自定义事件和变量 API

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，进行[数据分析](../../data-analytics/)。除自动收集的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](../../data-definition/custom-event/)和[变量]()，下面介绍自定义事件和变量 API 使用方法，后文简称埋点事件API。

### API 简介 

```java
// 发送自定义事件 API
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, JSONObject eventLevelVariables);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);

// 发送转化变量 API
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar(String key, String value);
gio.setEvar(String key, Number value);
gio.setEvar(String key, Boolean value);
gio.setEvar(JSONObject conversionVariables);

// 发送用户变量 API
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);

// 设置登录用户ID API
GrowingIO.getInstance().setUserId(String userId);

// 清除登录用户ID API
GrowingIO.getInstance().clearUserId();

// 设置访问用户变量，或者设置 A/B 测试标签
GrowingIO.getInstance().setVisitor(JSONObject visitorVariable);
```

### track

发送一个自定义事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

```java
// track API原型
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);
```

**参数说明：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">参数类型</th>
      <th style="text-align:left">必填</th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">事件标识符</td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">Number</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>事件的数值，没有number参数时，事件默认加一；</p>
        <p>当出现number参数时，事件自增number的数值</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">JSONObject</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">事件发生时所伴随的维度信息</td>
    </tr>
  </tbody>
</table>**参数限制条件：**

参数违反以下条件将不发送数据，`SDK 2.4.0` 以上能够在 Log 日志中查看对应报错，之下版本无提示信息。调用后请关注日志，查看数据发送是否成功，事件类型`t`为`cstm`。

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">
        <p>非空，长度限制小于等于50；</p>
        <p><code>SDK 2.4.0</code>以下版本不支持中文，仅支持 0 到 9、a 到 z 以及下划线，并且不能以数字开头。</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">非空。</td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">
        <p>非空，长度限制小于等于100（<code>eventLevelVariable.length()&lt;=100</code>）；</p>
        <p><code>eventLevelVariable</code> 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

```java
// track API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.track("registerSuccess");
```

```java
// track API调用示例二
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.track("registerSuccess", jsonObject);
```

```java
// track API调用示例三
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.track("loanAmount", 80000, jsonObject);
```

**检验数据发送日志示例：** 

注意 `t` 等于 `cstm` 字段，表示自定义事件发送成功，只需注意 `var`、`n` 、`num`字段，其它字段无需仔细验证**。**

```javascript
//展示 track 接口调用示例三日志内容
{
    "s":"31e3aa14-5241-490c-821c-a741e9bf0f87",
    // t 为事件类型， track 接口调用发送的事件类型为 cstm
    "t":"cstm",
    "tm":1532085495251,
    "d":"com.growingio.android.test",
    // n 为 eventId 参数携带的值
    "n":"loanAmount",
    // var 为 eventLevelVariable 参数携带的值
    "var":{
        "gender":"male",
        "age":"21"
    },
    "ptm":0,
    // num 为 number 参数携带的值
    "num":80000,
    "gesid":18,
    "esid":0,
    "u":"b6247b01-a31a-3bc6-a391-4c456888c1ee"
}
```

{% hint style="info" %}
#### 推荐您使用MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[cstm 事件验证](../growingio-debugger/best-practice.md#cstm-shi-jian-yi-ji-guan-lian-de-shi-jian-ji-bian-liang-shi-jian)。
{% endhint %}



### setEvar

转化变量是一种非常强大的变量类型，主要是为了归因而用，比如访问渠道、站外搜索关键词、站内搜索关键词等等。在 GrowingIO 里面可以定制变量的归因方式和持久性范围。

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

```java
// setEvar API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar(String key, String value);
gio.setEvar(String key, Number value);
gio.setEvar(String key, Boolean value);
gio.setEvar(JSONObject conversionVariables);
```

**参数说明：**

| 参数名称 | 参数类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 转化变量的标识符 |
| value | String | 否 | 转化变量的值 |
| conversionVariables | JSONObject | 否 | 转化变量用于高级归因分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">非空，长度限制小于等于50。</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">非空，长度限制小于等于1000。</td>
    </tr>
    <tr>
      <td style="text-align:left">conversionVariables</td>
      <td style="text-align:left">
        <p>非空，长度限制小于等于100（<code>conversionVariables.length()&lt;=100</code>）；</p>
        <p><code>conversionVariables</code> 内部不允许含有<code>JSONObject</code>或者<code>JSONArray</code>；</p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code>长度限制小等于1000。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

```java
// setEvar API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar("campaignId", "1234567890");
```

```java
// setEvar API调用示例二
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("campaignId", "1234567890");
jsonObject.put("campaignOwner", "Li Si");
gio.setEvar(jsonObject);
```

**检验数据发送日志示例：** 

注意 `t` 等于`evar`字段，表示转化事件发送成功，只需注意 `var`字段，其它字段无需仔细验证。

```javascript
{
    "s":"e1c48845-dd60-4cf2-b1a5-a8e529d2188d",
    // t 为事件类型， evar 为转化事件
    "t":"evar",
    "tm":1532338526083,
    "d":"com.growingio.android.test",
    "cs1":"GrowingIO",
    // 转化变量
    "var":{
        "evarTest":111,
        "campaignId":"1234567890",
        "campaignOwner":"Li Si"
    },
    "gesid":300,
    "esid":22
}
```

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ evar 事件验证](../growingio-debugger/best-practice.md#evar-zhuan-hua-bian-liang-shi-jian)
{% endhint %}

### setPeopleVariable

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明用户变量。

```java
// people.set API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);
```

**参数说明：**

| 参数名称 | 参数类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| peopleVariables | JSONObject | 否 | 用户变量用于用户信息相关的分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">非空，长度限制小于等于50。</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">非空，长度限制小于等于1000。</td>
    </tr>
    <tr>
      <td style="text-align:left">peopleVariables</td>
      <td style="text-align:left">
        <p>非空，长度限制小于等于100（<code>peopleVariables.length()&lt;=100</code>）；</p>
        <p><code>peopleVariables</code> 内部不允许含有<code>JSONObject</code>或者<code>JSONArray</code>；</p>
        <p><code>key</code>长度限制小于等于50，<code>value</code>长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

```java
// people.set API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable("gender", "male");
```

```java
// people.set API调用示例二
GrowingIO gio = GrowingIO.getInstance();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setPeopleVariable(jsonObject);
```

**检验数据发送日志示例：** 

注意 `t` 等于`ppl`字段，表示用户变量发送成功，只需注意 `var`字段，其它字段无需仔细验证。

```javascript
{
    "s":"a35872af-13df-4479-90bc-25558d12328e",
    // t 为事件类型， pvar 为发送用户变量事件
    "t":"ppl",
    "tm":1532339208991,
    "d":"com.growingio.android.test",
    "cs1":"GrowingIO",
    // 用户变量
    "var":{
        "gender":"male",
        "age":"21"
    },
    "gesid":311,
    "esid":0
}
```

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ ppl 事件验证](../growingio-debugger/best-practice.md#ppl-yong-hu-bian-liang-shi-jian)
{% endhint %}

### 

### setUserId

当用户登录之后调用setUserId API，设置登录用户ID。

```java
GrowingIO.getInstance().setUserId(String userId);
```

**参数说明：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">参数类型</th>
      <th style="text-align:left">必填</th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">userId</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">
        <p>登录用户Id，长度限制小于等于1000；</p>
        <p>如果值为空则清空了登录用户变量，不建议这么用，</p>
        <p>请使用 clearUserId 清除登录用户变量。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

```java
GrowingIO.getInstance().setUserId("1234567890");
```

注：您的 App 每次用户升级版本时无需重新登录的话，建议在用户每次升级App 版本后初次访问时重新调用上述 setUserId 方法。

{% hint style="warning" %}
#### 1. 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ 用户变量](../growingio-debugger/best-practice.md#chang-jing-yi-yong-hu-bian-liang-zhi-deng-lu-yong-hu-id)

#### 2. 如果要清除用户变量，请使用 [clearUserId](android-mai-dian-sdk.md#clearuserid)
{% endhint %}

### 

### clearUserId

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

**示例代码：**

```java
GrowingIO.getInstance().clearUserId();
```



### setVisitor

**2.4.0以上版本支持**

当用户未登录时，定义用户属性变量，也可用于A/B测试上传标签。

```java
GrowingIO.getInstance().setVisitor(JSONObject visitorVar)
```

**参数说明：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">参数类型</th>
      <th style="text-align:left">必填</th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">visitorVar</td>
      <td style="text-align:left">JSONObject</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">
        <p>不可使用嵌套的<code>JSONObject</code>对象，即为JSONObject中不可以放入<code>JSONObject</code>或者<code>JSONArray</code>；</p>
        <p>key 长度限制小于等于50，value长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

```java
GrowingIO gio = GrowingIO.getInstance();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setVisitor(jsonObject);
```

**检验数据发送日志示例：** 

注意 `t` 等于`vstr`字段，表示访问用户变量发送成功，其它字段无需仔细验证。

```javascript
{
    "s":"d334b4a1-57eb-4bf4-b426-64c1cce5a5c0",
    // t 为事件类型， vstr 为发送访问用户变量事件
    "t":"vstr",
    "tm":1532341259134,
    "d":"com.growingio.android.test",
    "cs1":"GrowingIO",
    //访问用户变量
    "var":{
        "gender":"male",
        "age":"21"
    },
    "gesid":322,
    "esid":0
}
```

## 验证 SDK 是否正常工作

验证 SDK 的采集事件数据发送至关重要，数据采集是数据分析的基础，保证了数据采集的精准才能帮助您做出正确的数据分析。

强烈建议开发者验证 GrowingIO SDK 的数据发送，如果有任何问题，及时反馈技术支持，我们会一直努力增强我们的采集能力，给您更好的服务。

### GrowingIO 采集事件介绍

GrowingIO 的数据采集分为自动采集和用户自定义事件和变量两种方式采集移动端APP，也就是通常所说的无埋点和埋点。下面将分别介绍这两种方式GrowingIO定义的采集事件类型。

#### 自动采集事件类型

<table>
  <thead>
    <tr>
      <th style="text-align:left">事件类型</th>
      <th style="text-align:left">含义</th>
      <th style="text-align:left">发送时机</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">activate</td>
      <td style="text-align:left">激活</td>
      <td style="text-align:left">当APP首次激活打开时</td>
    </tr>
    <tr>
      <td style="text-align:left">vst</td>
      <td style="text-align:left">应用访问</td>
      <td style="text-align:left">
        <p>1.冷启动发送</p>
        <p>2.切换用户发送</p>
        <p>3.默认APP进入后台30秒以后再次打开会发送</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">reengage</td>
      <td style="text-align:left">DeepLink唤醒事件</td>
      <td style="text-align:left">通过扫描 DeepLink 二维码唤醒 APP 后发送</td>
    </tr>
  </tbody>
</table>#### 自定义事件类型

| 事件类型 | 含义 | 发送时机 |
| :--- | :--- | :--- |
| cstm | 自定义事件触发 | 调用[`track`](android-sdk.md#track)后发送 |
| evar | 转化变量触发 | 调用[`setEvar`](android-sdk.md#setevar)后发送 |
| ppl | 设置用户变量 | 调用[`setPeopleVariable`](android-sdk.md#setpeoplevariable)后发送 |
| vstr | 设置访问用户变量 | 调用[`setVisitor`](android-sdk.md#setvisitor)后发送 |



### 验证工具

#### [1. MobileDebugger](../growingio-debugger/#qi-dong-mobile-debugger)

#### [2.查看日志](android-sdk.md#setdebugmode)



## Android 埋点 SDK API

GrowingIO 提供了初始化配置项 API 和运行时 API 来自定义 SDK 的采集，满足不同场景的定制采集。同一种含义的API，只需要调用一次即可，比如您关闭SDK的采集功能，可以在初始化中配置，也可以在运行时配置，只需调用一次相关API即可。

{% hint style="danger" %}
**GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据，对比无埋点 SDK ，埋点 SDK 少很多 API， 请勿在埋点 SDK 中调用无埋点 SDK 接口。**
{% endhint %}

### 埋点SDK初始化配置项 API

初始化配置项均在`Application`的`onCreate`方法中 SDK 初始化代码块中设置，下面将分类并描述含义。

#### 示例代码

```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        GrowingIO.startWithConfiguration(this,new Configuration()
            .setBulkSize(100)
            .setCellularDataLimit(1000)
            .setChannel("渠道号")
            .setDebugMode(true)
            .setDeeplinkCallback(new DeeplinkCallback() {
                @Override
                public void onReceive(Map<String, String> params, int error) {
        ​
                }
            })
            .setDiagnose(false)
            .setDisabled(false)
            .setFlushInterval(1000)
            .setSampling(0.34)
            .setSessionInterval(23000)
            .setTestMode(true)
            .setMutiprocess(true)
        );
    }
}
```

\*\*\*\*

### 基础配置 API

<table>
  <thead>
    <tr>
      <th style="text-align:left">初始化配置项API</th>
      <th style="text-align:left">默认值</th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">setTestMode</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">在Logcat中输出采集日志</td>
    </tr>
    <tr>
      <td style="text-align:left">setDebugMode</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">
        <p>实时发送数据，开启则不遵循移动网络状态下数据发送大小默认 3M 限制以及采集数据缓存30秒发送策略。</p>
        <p>为了方便开发者查看日志，一般和<code>setTestMode</code>一起使用。</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setChannel</td>
      <td style="text-align:left">无</td>
      <td style="text-align:left">设置渠道</td>
    </tr>
  </tbody>
</table>

### SDK 功能API

| 初始化配置项API | 默认值                    | 说明 |
| :--- | :--- | :--- |
| setDeeplinkCallback | 无 | DeepLink 回调接口，获得自定义参数以便跳转对应 APP页 面 |
| setMutiprocess | false | 使用了多进程必须配置，自定义事件和变量值才会多进程共享 |



### 数据采集发送 API

| 初始化配置项API | 默认值                                                        | 说明 |
| :--- | :--- | :--- |
| setDisabled | false | SDK 是否采集数据，设置为`true`时不采集数据 |
| setSampling | 1 | 采样率\[0.01~1\],若设置sampling = 0.01，则 1% 的设备会被采集数据，每次启动会根据用户设置的采样率判断设备是否在采集的范围之内，**使用之前请咨询技术支持** |
| setSessionInterval | 30 \* 1000 | 在后台停留时长超过此值，则产生新的`sessionId`,发送`visit`事件。 |
| setFlushInterval | 30 \* 1000 | 数据刷新的最长时间间隔，默认30 秒 。如果距离上次发送数据事件超过此时间则发送事件 |
| setCellularDataLimit | 3 \* 1024 \* 1024 | 一天的时间之内，在移动蜂窝网下的数据最大传输量，默认3M。 |
| setBulkSize | 300 | 如果数据库存储数据条数大于等于`bulkSize`，则马上发送数据。 |



## GrowingIO 运行时 API

GrowingIO 为 APP 提供运行时随意调用的 API，使用方法如下：

```java
// 得到 GrowingIO 实例后可以调用其中 API
GrowingIO gio = GrowingIO.getInstance();
gio.setUserId("张溪梦");
```

{% hint style="danger" %}
GrowingIO 所有 API 都需要在主线程调用。
{% endhint %}



### 基础配置 API

| 运行时API | 说明 |
| :--- | :--- |
| setGeoLocation | 设置经纬度，并在 vst 事件中发送，Android SDK 暂时没办法自动获取`GPS`数据，如果您要采集`GPS`数据，需要在您的App每次获取完`GPS`数据之后，通过该`API`告知 SDK。如果您不设置，我们默认使用用户`IP`的`Location。` |
| clearGeoLocation | 清空经纬度 |



### 数据采集 API 

| 运行时API | 说明 |
| :--- | :--- |
| disableDataCollect | 遵守欧洲联盟出台的通用数据保护条例，用户不授权，不采集用户数据 |
| enableDataCollect | 遵守欧洲联盟出台的通用数据保护条例，用户授权，采集用户数据 |
| disable | GrowingIO 停止采集 |
| resume | GrowingIO 恢复采集 |
| stop | GrowingIO 停止采集，可以不在主线程调用 |
| getSessionId | 得到 session id |
| getDeviceId | 获取设备id，对应数据采集的`u`字段，又称为`匿名用户id`，用来定义一台设备，SDK 自动生成。 |



### 自定义事件和变量API

在 Android SDK 文档中描述的更详细，请点击查看。

| 自定义事件和变量API | 说明 |
| :--- | :--- |
| track | 发送自定义事件，对应`cstm`事件 |
| setEvar | 发送转化变量，对应`evar`事件 |
| setPeopleVariable | 发送用户变量，对应`ppl`事件 |
| setUserId | 设置登录用户ID，对应`cs1`字段 |
| clearUserId | 清除登录用户ID |
| setVisitor | 设置访问用户变量，对应`vstr`事件 |

