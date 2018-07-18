# Android SDK

* [集成](android-sdk.md#ji-cheng)
  * [添加依赖](android-sdk.md#1-tian-jia-yi-lai)
  * [添加 URLScheme 和网络权限](android-sdk.md#2-tian-jia-urlscheme-he-wang-luo-quan-xian)
  * [初始化SDK](android-sdk.md#3-chu-shi-hua-sdk)
  * [代码混淆](android-sdk.md#4-dai-ma-hun-xiao)
* [SDK API](android-sdk.md#sdk-api)
  * [重要配置项 API](android-sdk.md#1-zhong-yao-pei-zhi-xiang-api)
  * [自定义事件和变量 API](android-sdk.md#2-zi-ding-yi-shi-jian-he-bian-liang-api)
* [旧版本升级](android-sdk.md#jiu-ban-ben-sheng-ji)
  * [更新 SDK 版本号为最新版本](android-sdk.md#1-geng-xin-sdk-ban-ben-hao-wei-zui-xin-ban-ben)
  * [迁移用户属性字段（CS字段）](android-sdk.md#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [迁移页面属性字段（PS字段）](android-sdk.md#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [迁移自定义事件（埋点事件）](android-sdk.md#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [数据校验](android-sdk.md#5-shu-ju-xiao-yan)
* [旧版本 SDK 集成 ](android-sdk.md#jiu-ban-ben-sdk-ji-cheng)

## 集成

### 1. 添加依赖

Gradle编译环境（AndroidStudio）

**\(1\)在project级别的build.gradle文件中添加vds-gradle-plugin依赖**

```groovy
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        //gradle建议版本
        classpath 'com.android.tools.build:gradle:3.1.3'
        classpath 'com.growingio.android:vds-gradle-plugin:2.3.3'
    }
}
```

**\(2\)在module级别的build.gradle文件中添加com.growingio.android插件、vds-android-agent依赖和对应的资源；**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种：

* 新产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt; 第二段中"此应用的 URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
* 现有产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![](../.gitbook/assets/image%20%284%29.png)

```groovy
apply plugin: 'com.android.application'
//添加插件
apply plugin: 'com.growingio.android'

android {
    defaultConfig {
        resValue("string", "growingio_project_id", "您的项目ID")
        resValue("string", "growingio_url_scheme", "您的URL Scheme")
    }
}
dependencies {
        compile 'com.growingio.android:vds-android-agent:2.3.3@aar'
}
```

### 2.添加URLScheme和网络权限

把URL Scheme添加到您的项目，以便我们唤醒您的程序，进行圈选。将该产品的URLScheme添加到你的AndroidManifest.xml中的LAUNCHER Activity下。例如：

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.growingio.testdemo">

    <!--请注意添加网络权限-->
    <uses-permission android:name="android.permission.INTERNET" />
    <!--非危险权限，不需要运行时请求，Manifest文件中添加即可-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

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

### 3. 初始化SDK

请将以下 GrowingIO.startWithConfiguration加在您的Application 的 onCreate 方法中

```java
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        GrowingIO.startWithConfiguration(this, new Configuration()
            .trackAllFragments()
            .setChannel("XXX应用商店")
        );
    }
}
```

（1）请确保将代码添加在`Application`的`onCreate`方法中，添加到其他方法中可能导致数据不准确。

（2）其中`GrowingIO.startWithConfiguration`第一个参数为`Application`对象。

（3）`trackAllFragments`方法用于把`Fragment`自动识别为页面，但一个界面中只能同时显示一个`Fragment`。

（4）`setChannel`方法的参数定义了“自定义App渠道”这个维度的值。

### 4. 代码混淆

如果你启用了混淆，请在你的proguard-rules.pro中加入如下代码：

```text
-keep class com.growingio.android.sdk.** {
    *;
}
-dontwarn com.growingio.android.sdk.**
-keepnames class * extends android.view.View
-dontwarn android.support.**
-keep class android.support.**{
    *;
}

-keep class com.growingio.android.sdk.collection.GrowingIOInstrumentation {
    public *;
    static <fields>;
}
```

如果您使用了AndResGuard,请在白名单里添加GrowingIO,如下：

```text
R.string.growingio*
```

**添加代码之后，请先Clean项目，然后再进行编译，并在你的 Android App 安装了 SDK 后重新启动几次 App，保证行为采集数据自动发送给 GrowingIO，以便顺利完成检测。**

## SDK API

### 1. 重要配置项 API

#### **\(1\) 采集H5页面数据**

如果您在App内嵌入了WebView（包括X5内核），请确保您已经调用过下面的方法，来采集H5页面的数据：

```java
WebView.setWebChromeClient(WebChromeClient client);
```

**请在第一次调用 WebView.loadUrl\(\)** 之前调用以上方法。

#### **\(2\) 采集Banner数据**

很多应用的界面上方都有横向滚动的 Banner 广告。

对于此类广告，如果您的应用通过 ViewPager、AdapterView 或者 RecyclerView 实现，请在 Banner创建时（包括动态创建）调用下面的接口来采集数据。

```java
GrowingIO.getInstance().trackBanner(banner, bannerDescriptions);
```

其中 bannerDescriptions 是 List&lt;String&gt;类型，包含所有广告图对应的广告内容描述，内容描述需要跟广告的顺序相同。

例如，当您有 5 张广告图时，只需创建一个 String 类型的 List，然后按 5 个广告出现的顺序给 List 的元素设置对应的广告描述，同样设置 5 个元素即可。

#### **\(3\) 采集GPS数据**

如果您需要采集用户的GPS数据，请在获取坐标后，调用如下接口进行设置

```java
GrowingIO.getInstance().setGeoLocation(latitude, longitude);
```

其中，`latitude`是纬度，`longitude`是经度。

当用户下一次切换页面，或者发生点击行为时，GPS数据会被发送回GrowingIO。

如果您需要清除用户的GPS信息，请调用如下接口

```java
GrowingIO.getInstance().clearGeoLocation();
```

#### **\(4\) 采集输入框数据**

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置

```java
GrowingIO.getInstance().trackEditText(EditText);
```

其中，`EditText`代表要被采集的输入框。

当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。

请注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。

#### **\(5\) 启用Hashtag识别**

在 SDK 初始化方法中设置

```java
GrowingIO.startWithConfiguration(this, new Configuration().setHashTagEnable(true));
```

#### **\(6\) 多进程支持**

SDK默认不支持多进程使用， 但是可以通过confiuration 进行设置支持多进程。 设置方法为， 在需要使用SDK功能的进程的Application onCreate中初始化SDK：

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                  .supportMultiProcessCircle(true)
                  .setMutiprocess(true)
                  );
```

其中supportMultiProcessCircle 与 setMutiprocess要同时使用， 而且多个进程中设置的值要相同。

**为什么不默认支持多进程？**

跨进程通信是一个相对较慢的过程， 默认不开启， 可以满足大部分用户的要求。

**哪些进程需要初始化SDK？**

需要使用SDK功能的进程需要初始化SDK， 所有的UI进程 + 部分Service进程\(如果这些进程中涉及手动打点\)。

#### **\(7\) 调试查看发送数据日志**

为了方便开发人员调试 GrowingIO SDK ，我们提供了调试 API 和 [Mobile Debugger](growingio-debugger/#growingio-mobile-debugger) 两种方式查看 APP 采集数据的发送，强烈建议您使用 Mobile Debugger 在每一次产品上线前检查您的数据发送，其中调试 API 使用说明如下：

`setDebugMode(true)` ：查看数据采集发送日志，默认值为`false`不开启；

`setTestMode(true)`：实时发送数据，不遵循移动网络状态下数据发送大小限制和缓存策略，默认值为`false`不开启；

**注意在 APP 上线前关闭，否则会导致采集数据日志打印，并且采集数据发送不遵循缓存策略**，代码如下：

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                  .setDebugMode(true)
                  .setTestMode(true)
                  );
```

### 2. 自定义事件和变量 API

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，进行[数据分析](../data-analytics/)。除自动收集的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](../data-defination/events-metrics/manual-metrics.md)和[变量](../data-defination/dimensions/manual-dimensions.md)，下面介绍自定义事件和变量 API 使用方法。

#### API 简介

```java
// 发送事件 API
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);

// 发送页面级变量 API
GrowingIO gio = GrowingIO.getInstance();
gio.setPageVariable(Activity activity, String key, String value);
gio.setPageVariable(Activity activity, String key, Number value);
gio.setPageVariable(Activity activity, String key, Boolean value);
gio.setPageVariable(Activity activity, JSONObject pageLevelVariables);

gio.setPageVariable(Fragment fragment, String key, String value);
gio.setPageVariable(Fragment fragment, String key, Number value);
gio.setPageVariable(Fragment fragment, String key, Boolean value);
gio.setPageVariable(Fragment fragment, JSONObject pageLevelVariables);

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
```

#### track

发送一个事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加一；当出现number参数时，事件自增number的数值 |
| eventLevelVariable | JSONObject | 否 | 事件发生时所伴随的维度信息 |

```java
// track API原型
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);
```

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

#### setPageVariable

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSONObject | 否 | 页面级别的信息 |

```java
// setPageVariable API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setPageVariable(Activity activity, String key, String value);
gio.setPageVariable(Activity activity, String key, Number value);
gio.setPageVariable(Activity activity, String key, Boolean value);
gio.setPageVariable(Activity activity, JSONObject pageLevelVariables);

gio.setPageVariable(Fragment fragment, String key, String value);
gio.setPageVariable(Fragment fragment, String key, Number value);
gio.setPageVariable(Fragment fragment, String key, Boolean value);
gio.setPageVariable(Fragment fragment, JSONObject pageLevelVariables);
```

```java
// page.set API调用示例
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setPageVariable(myActivity, jsonObject);
```

#### setEvar

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 转化变量的标识符 |
| value | String | 否 | 转化变量的值 |
| conversionVariables | JSONObject | 否 | 转化变量用于高级归因分析 |

```java
// setEvar API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar(String key, String value);
gio.setEvar(String key, Number value);
gio.setEvar(String key, Boolean value);
gio.setEvar(JSONObject conversionVariables);
```

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

#### setPeopleVariable

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSONObject | 否 | 用户变量用于用户信息相关的分析 |

```java
// people.set API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);
```

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

#### setUserId

当用户登录之后调用setUserId API，设置登录用户ID。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- |
| userId | String | 是 | 登录用户I，长度限制1000 |

```java
// setuserId API调用示例
GrowingIO.getInstance().setUserId("1234567890");
```

注：如果您的应用是App，且每次用户升级App版本时无需重新登录的话，建议在用户每次升级App版本后初次访问时重新调用上述 setUserId 方法。

#### clearUserId {#clearuserid}

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

```java
// clearUserId API调用示例
GrowingIO.getInstance().clearUserId();
```



## 旧版本升级

本文旨在帮助您从 1.x 版本无缝升级至 2.x 版本，由于两个版本的诸多接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

您需要完成以下几个步骤：

### 1. 更新 SDK 版本号为最新版本

请您参考以下开发文档，完成SDK初始化代码的添加。

* [Android SDK 集成](android-sdk.md#ji-cheng)

Tips：建议您在开发中，使用 `DebugMode` 或者 [**Mobile Debugger**](growingio-debugger/#shi-yong-mobile-debugger-ce-shi-shu-ju) 校验 GrowingIO SDK 的数据是否正常上传，其中开启`DebugMode`的方式见[调试查看发送数据日志](android-sdk.md#7-tiao-shi-cha-kan-fa-song-shu-ju-ri-zhi)。

### **2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至[“用户变量”](../data-defination/dimensions/manual-dimensions.md#yong-hu-bian-liang)。两者的区别主要在于：用户变量支持自定义的归因方式。

**2.1 上传接口：**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

* 1.x 版本方法格式：

```java
GrowingIO growingIO = GrowingIO.getInstance();
    growingIO.setCS1("user_id", "100324");
    growingIO.setCS2("company_id", "943123");
    growingIO.setCS3("user_name", "张溪梦");
```

* 2.x 版本方法格式：

对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```java
// 设置登录用户ID API
GrowingIO.getInstance().setUserId(String userId);

// 清除登录用户ID API
GrowingIO.getInstance().clearUserId();
```

对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```java
GrowingIO.setAppVariable(JSONObject variables)
GrowingIO.setAppVariable(String key, Number variable)
GrowingIO.setAppVariable(String key, String variable)
GrowingIO.setAppVariable(String key, Boolean variable)
```

对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```java
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);// 多个变量，可组合为一个JSON对象peopleVariables传入
```

* 2.x 版本方法格式：

```java
GrowingIO gio = GrowingIO.getInstance();
gio.setPageVariable(Activity activity, String key, String value);
gio.setPageVariable(Activity activity, String key, Number value);
gio.setPageVariable(Activity activity, String key, Boolean value);
gio.setPageVariable(Activity activity, JSONObject pageLevelVariables);

gio.setPageVariable(Fragment fragment, String key, String value);
gio.setPageVariable(Fragment fragment, String key, Number value);
gio.setPageVariable(Fragment fragment, String key, Boolean value);
gio.setPageVariable(Fragment fragment, JSONObject pageLevelVariables);
```

**2.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-defination/dimensions/manual-dimensions.md#ye-mian-ji-bian-liang)。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量](../data-defination/dimensions/manual-dimensions.md#ye-mian-ji-bian-liang)”。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

**3.1 上传接口**

* 1.x 版本方法格式：

```java
GrowingIO.setPageGroup(Activity activity, String name); 
GrowingIO.setPS1(Activity activity, String property); 
GrowingIO.setPS2(Activity activity, String property);
```

* 2.x 版本方法格式：

```java
GrowingIO gio = GrowingIO.getInstance();
gio.setPageVariable(Activity activity, String key, String value);
gio.setPageVariable(Activity activity, String key, Number value);
gio.setPageVariable(Activity activity, String key, Boolean value);
gio.setPageVariable(Activity activity, JSONObject pageLevelVariables);

gio.setPageVariable(Fragment fragment, String key, String value);
gio.setPageVariable(Fragment fragment, String key, Number value);
gio.setPageVariable(Fragment fragment, String key, Boolean value);
gio.setPageVariable(Fragment fragment, JSONObject pageLevelVariables);
```

  
**3.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-defination/dimensions/manual-dimensions.md#ye-mian-ji-bian-liang)。

### 4. 迁移自定义事件（埋点事件）

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

**4.1 上传接口**

```java
public class GrowingIO {
    public GrowingIO track(String eventName, JSONObject properties)
}
```

* 2.x 版本方法格式：

```java
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);
```

**4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在**“管理” - “自定义事件和变量”** 页面中的 **“自定义事件” Tab 页**。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

### 5. 数据校验

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。[点击查看 GrowingIO Mobile Debugger 的安装和使用](growingio-debugger/#growingio-mobile-debugger)。

## 旧版本 SDK 集成

从0.9.85版（2016年7月20日发布）起，SDK的集成方式发生变化，下方文档已经更新。

注意：目前 Android SDK **不支持 Instant Run**，原因是 SDK 采用的 Hook 方案是字节码植入方案，植入是在编译时期，但是因为 Instant Run 是不完全编译所以会造成新更改的元素无法正常 Hook。如果您想要测试效果或者发版，采用一次全新编译即可。

#### 1. 导入SDK

Gradle 编译环境（Android Studio）

**一、在project级别的build.gradle文件中添加vds-gradle-plugin依赖：**

```groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'
        classpath 'com.growingio.android:vds-gradle-plugin:1.1.9'
    }
}
```

**二、在module级别的build.gradle文件中添加com.growingio.android插件、vds-android-agent依赖和对应的资源：**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种

1. 添加新产品：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt; 第二段中"此应用的 URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
2. 现有产品：登录官网 -&gt; 点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![](../.gitbook/assets/image%20%2831%29.png)

```groovy
apply plugin: 'com.android.application'
//添加 GrowingIO plugin
apply plugin: 'com.growingio.android'
android {
    defaultConfig {
        resValue("string", "growingio_project_id", "您的项目ID")
        resValue("string", "growingio_url_scheme", "您的URL Scheme")
    }
} 
dependencies {
        compile 'com.growingio.android:vds-android-agent:1.1.9@aar'
}
```

2. 添加URL Scheme

将下面的启动圈选接口添加到您的`AndroidManifest.xml`中的LAUNCHER `Activity`下以便我们唤醒您的程序，进行圈选。

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.growingio.testdemo">

    <!--请注意添加网络权限-->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

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

**请添加一整个intent-filter区块,并确保其中只有一个data字段。**

3. 初始化SDK

请将以下 `GrowingIO.startWithConfiguration`加在您的`Application` 的 `onCreate` 方法中

```java
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        GrowingIO.startWithConfiguration(this, new Configuration()
                .trackAllFragments()
                .setChannel("XXX应用商店")
                .setDebugMode(true); //打开调试Log，注意上线关闭设置为 false
    }
}
```

1. 请确保将代码添加在`Application` 的 `onCreate` 方法中，添加到其他方法中可能导致数据不准确。
2. 其中`GrowingIO.startWithConfiguration`第一个参数为 `Application` 对象。
3. 对于已经集成过旧版SDK并圈选过的应用，调用`useID`会导致新圈选的指标数值从零开始计算，类似初次集成SDK后发版的效果，但不影响之前圈选的指标数据。如果不希望出现这种情况，请去掉这个方法的调用。
4. `trackAllFragments`方法用于把`Fragment`自动识别为页面，但一个界面中只能同时显示一个`Fragment`。
5. `setChannel`方法的参数是渠道的名称。

#### 4. 代码混淆

1. 如果您启用了代码混淆，请在您的 proguard-rules.pro 中添加以下代码

```text
-keep class com.growingio.android.sdk.** {
    *;
}
-dontwarn com.growingio.android.sdk.**
-keepnames class * extends android.view.View
-dontwarn android.support.**
-keep class android.support.**{
    *;
}

-keep class com.growingio.android.sdk.collection.GrowingIOInstrumentation {
    public *;
    static <fields>;
}
```

如果您使用了AndResGuard,请在白名单里添加GrowingIO,如下：

```text
R.string.growingio*
```

**添加代码之后，请先Clean项目，然后再进行编译，并在你的 Android App 安装了 SDK 后重新启动几次 App，以保证行为采集数据自动发送给 GrowingIO，完成检测。**

####  {#下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要}

#### 5. 重要配置项

**下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要**

**采集H5页面数据**

如果您在App内嵌入了WebView（包括X5内核），请确保您已经调用过下面的方法，来采集H5页面的数据：

```java
WebView.setWebChromeClient(WebChromeClient client);
```

请在第一次调用`WebView.loadUrl()`之前调用以上方法。

**采集广告Banner数据**

很多应用的界面上方都有横向滚动的Banner广告。

对于此类广告，如果您的应用通过ViewPager、AdapterView或者RecyclerView实现，请在Banner创建时（包括动态创建）调用下面的接口来采集数据。

```java
GrowingIO.getInstance().trackBanner(banner, bannerDescriptions)
```

其中`bannerDescriptions`是`List<String>`类型，包含所有广告图对应的广告内容描述，内容描述需要跟广告的顺序相同。

例如，当您有5张广告图时，只需创建一个`String`类型的`List`，然后按5个广告出现的顺序给List的元素设置对应的广告描述，同样设置5个元素即可。

#### 页面别名 {#页面别名}

对于安卓应用，页面指的是`Activity`或者`Fragment`。

有些时候，对于完成某个功能的页面，统计时可能需要进一步细分。 比如，对于展示商品列表的页面，需要区分衣物类商品，以及食品类商品的两种列表的访问量。

为处理这种场景，我们提供了取别名的方法来区分这两种情况下的页面，方法如下：

```java
GrowingIO.setPageName(Activity activity, String name)
```

如果您设置的的对象是`Fragment`，将上方的`Activity`替换为`Fragment`即可。

**我们用**`Activity`**来举例，具体说明它的用法。**

1. 某个应用的商品列表页是用`FeedActivity`实现的，所以默认的页面名称都是`FeedActivity`。
2. 现在我们想区分衣物类商品列表和食品类商品列表，分别看它们的浏览量，可以在`onCreate`方法中添加如下代码：

   ```java
   public class FeedActivity extends Activity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            GrowingIO.getInstance().setPageName(this, "Clothing");
        }
   }
   ```

请注意

1. 必须在该`Activity`的`onCreate`方法中完成该属性的赋值操作。
2. 页面别名只能设置为字母、数字和下划线的组合。
3. 为查看数据方便，请尽量对iOS和安卓的同功能页面取不同的名称。

至此集成完毕。您需要在【添加新产品】页面进行检测并安装。安装成功后您可以在您的App中激活SDK。

#### 采集Fragment数据 {#采集fragment数据}

当一个`Activity`下同时在屏幕中显示多个`Fragment`时，我们无法判断将哪个作为页面统计。

如果您的应用中有这种情况，请在初始化时删除对于`Configuration.trackAllFragments`的调用，这样就不会默认采集任何`Fragment`的数据。

然后使用下面的接口，在`Activity`中指定某一个`Fragment`作为页面进行数据采集：

```java
GrowingIO.trackFragment(Activity activity, Fragment fragment)
```

请在`Fragment`添加到`Activity`之前调用此接口，每个`Fragment`对象调用一次即可。

#### 采集GPS\(地理位置信息\)数据 {#采集gps地理位置信息数据}

注：Android SDK暂时没办法自动获取GPS数据，如果您要采集GPS数据，需要在您的App每次获取完GPS数据之后，通过以下Api告知SDK。  
在您的App获取GPS坐标后，调用如下接口进行设置

```java
GrowingIO.getInstance().setGeoLocation(latitude, longitude);
```

其中，`latitude`是纬度，`longitude`是经度。

当用户下一次切换页面，或者发生点击行为时，GPS数据会被发送回GrowingIO。

如果您需要清除用户的GPS信息，请调用如下接口

```java
GrowingIO.getInstance().clearGeoLocation();
```

#### 采集输入框数据 {#采集输入框数据}

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置

```java
GrowingIO.getInstance().trackEditText(EditText);
```

其中，`EditText`代表要被采集的输入框。

当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。

请注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。

#### 渠道设置 {#渠道设置}

为方便用户生成不同渠道的安装包，我们除了允许在Configuration的setChannel方法中设置渠道以外还允许在AndroidManifest.xml中通过MetaData设置安装包渠道：

```markup
<meta-data android:name="com.growingio.android.GConfig.Channel" android:value="Your ChannelID"/>
```

#### 启用Hashtag识别 {#启用hashtag识别}

对于 1.1.4 及以上 SDK 版本，在 SDK 初始化方法中设置

```java
GrowingIO.startWithConfiguration(this, new Configuration().setHashTagEnable(true)）
```

#### 6. 其他配置项

#### 自定义维度 {#自定义维度}

GrowingIO的数据分析工具提供了例如“应用版本”，“渠道”，“城市”，“设备型号”等等这些通用维度。但在使用过程中，有时无法满足用户对特定数据维度的分析要求。

为了能够让数据分析变得更加的灵活，我们提供了自定义维度的接口，使用者可以对各种对象设置属性。

**这些属性在作图时，将表现为维度。**

**用户属性**

用户属性只能用来表示登录用户本身的属性，至少包括用户ID。

根据需求，可以用来指定用户的各种属性

1. 自然属性，比如性别、出生年月等。
2. 账户属性，比如等级、类型标签等。
3. 行为特征，比如是否有过购买记录之类。

用户属性被称为CS字段，最多支持十个，从CS1到CS10，接口如下：

```java
GrowingIO growingIO = GrowingIO.getInstance();
growingIO.setCS1("CS1的key", "CS1的value");
growingIO.setCS2("CS2的key", "CS2的value");
...
growingIO.setCS10("CS10的key", "CS10的value");
```

在下面的例子中，总计上传5个用户属性，分别是：

CS1: user\_id:100324  
CS2: company\_id:943123  
CS3: user\_name:张溪梦  
CS4: company\_name:GrowingIO  
CS5: sales\_name:销售员小王

```java
private void setGrowingIOCS() {
    GrowingIO growingIO = GrowingIO.getInstance();
    growingIO.setCS1("user_id", "100324");
    growingIO.setCS2("company_id", "943123");
    growingIO.setCS3("user_name", "张溪梦");
    growingIO.setCS4("company_name", "GrowingIO");
    growingIO.setCS5("sales_name", "销售员小王");
}
```

**CS字段设置条件和限制**

1. CS 字段不能是和用户没有直接关系的属性，比如不能是订单 ID，商品 ID 等。
2. CS1 字段：在 GrowingIO 系统中用于识别注册用户的身份，因此 CS1 的 value 必须填写用户的唯一身份标示 ID。
3. CS2 字段：在 GrowingIO 系统中用于识别 SaaS 客户的租户，因此所有的 SaaS 用户必须填写租户的唯一身份标示 ID，非 SaaS 用户不做限定。
4. 对于未登录用户，不要设置任何CS字段。
5. 如果没有用到所有的CS字段，剩下的可以不设置。
6. 同一个CS字段，必须保持在各个平台意义相同。

**CS1字段设置时机**

基本原则：当App使用者的登录状态改变时设置CS1字段的值

用户手动登录 1. 如果有多个登录入口，在每一个入口登录成功后，都需要调用`GrowingIO.setCS1`来设置用户的唯一标识 2. 如果有第三方登录，成功登录后需要调用`GrowingIO.setCS1`方法

自动登录：App启动时，自动登录或者用户默认是登录状态，也需要调用`GrowingIO.setCS1`方法

注册：有的App注册成功后，默认登录，这种情况下也需要调用`GrowingIO.setCS1`方法

退出：退出登录后，请勿继续上传CS字段，即使是空值也请勿上传。

**其他CS字段遵循相似的设置时机**

**在上传成功两小时后，您需要在「项目管理-项目配置-CS 配置中」进行字段配置和激活，配置成功后便可开始使用 CS 字段进行分析。**

#### 设置界面元素内容 {#设置界面元素内容}

SDK默认不会采集ImageView的内容，如果您需要区分不同的ImageView，可以使用contentDescription来标记，也可以调用下方的方法来设置：

```java
GrowingIO.setViewContent(View view, String content);
```

#### 设置界面元素ID {#设置界面元素id}

调用了`useID`方法后，SDK将会使用Layout文件中的ID来识别一个元素。

如果部分元素在Layout文件中没有ID，建议在Layout文件中添加。

对于动态生成的元素，可以使用如下方法对它设置唯一的ID：

1. 调用GrowingIO.setViewID\(View view, String viewID\)。第一个参数是要设置的View，第二个是给这个View的ID。
2. 如果在ViewGroup上设置ID的话，SDK会忽略他所有子元素的默认ID（就是写在xml文件里的）只会使用GrowingIO.setViewID设置的ID。
3. ID只能设置为字母、数字和下划线的组合。

#### 忽略元素 {#忽略元素}

如果您需要忽略某些特殊内容，比如倒计时元素或涉及隐私的内容，可以使用该功能。

```java
GrowingIO.ignoreView(View view)
```

#### 设置元素对象 {#设置元素对象}

如果元素自身的内容并不能代表具体的意义，可以使用元素对象来标识。

例如“加入购物车”按钮，可以设置成加入购物车的具体商品名称或ID。

```java
GrowingIO.setViewInfo(View view, String info);
```

#### 动态添加View {#动态添加view}

如果您有某些View动态添加到ViewTree中并且在父容器中的位置不固定（例如常见的多Fragment实现的Tab切换），请给每个View设置ID来辅助统计

```java
View content = getLayoutInflater().inflate(R.layout.content_view, container, false);

GrowingIO.setTabName(content, "MyContent");
```

#### 自定义点击事件 {#自定义点击事件}

如果您有自定义的控件重写了`View`的`onTouchEvent`方法来判断和处理点击事件，那么必须调用它的`PerformClick`，并且设置相应的`onClickListener`。

至此，您的SDK安装就成功了。您登录 GrowingIO 进入产品安装页面执行“数据检测”几分钟后就可以看到数据了。

  


