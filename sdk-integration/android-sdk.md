# Android SDK

* [SDK 代码安装](android-sdk.md#sdk-安装)
* [Android SDK API](android-sdk.md#android-sdk-api)
* [自定义数据上传&配置指导](android-sdk.md#自定义数据上传配置指导)
* [1.x 版本 SDK 升级指导](android-sdk.md#1x-版本-sdk-升级指导)
* [GrowingIO Mobile Debugger](android-sdk.md#growingio-mobile-debugger)

### SDK 代码安装 {#sdk-安装}

重要：如果您目前已经在使用GrowingIO 1.x 版本的 SDK，希望升级到最新版本 \(V2.1），您需要联系您的 GrowingIO 对接人，我们需要帮您开启新版本 SDK 的功能。如果您忽略该提醒自助升级，而后台对应功能未开启的话，可能会带来数据丢失的问题。如何升级?

#### 1.导入SDK {#1导入sdk}

Gradle编译环境（AndroidStudio）

**\(1\)在project级别的build.gradle文件中添加vds-gradle-plugin依赖**

```text
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        //gradle建议版本
        classpath 'com.android.tools.build:gradle:3.0.1'
        classpath 'com.growingio.android:vds-gradle-plugin:2.3.3'
    }
}
```

**\(2\)在module级别的build.gradle文件中添加com.growingio.android插件、vds-android-agent依赖和对应的资源；**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种：

* 新产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt; 第二段中"此应用的 URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
* 现有产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![&#x9879;&#x76EE;&#x7BA1;&#x7406;](https://docs.growingio.com/.gitbook/assets/image%20%281%29.png)

```text
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

#### 2.添加URLScheme和网络权限 {#2添加urlscheme和网络权限}

把URL Scheme添加到您的项目，以便我们唤醒您的程序，进行圈选。将该产品的URLScheme添加到你的AndroidManifest.xml中的LAUNCHER Activity下。例如：

```text
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

#### 3. 初始化SDK {#3-初始化sdk}

请将以下 GrowingIO.startWithConfiguration加在您的Application 的 onCreate 方法中

```text
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        GrowingIO.startWithConfiguration(this, new Configuration()
        .useID()
        .trackAllFragments()
        .setChannel("XXX应用商店"))
        .setDebugMode(true); //打开调试Log
    }
}
```

（1）请确保将代码添加在`Application`的`onCreate`方法中，添加到其他方法中可能导致数据不准确。

（2）其中`GrowingIO.startWithConfiguration`第一个参数为`Application`对象。

（3）使用`useID`方法，能够更准确地统计界面元素，一般建议添加。

（4）`trackAllFragments`方法用于把`Fragment`自动识别为页面，但一个界面中只能同时显示一个`Fragment`。

（5）`setChannel`方法的参数定义了“自定义App渠道”这个维度的值。

添加代码之后，**请先 Clean 项目**，然后再进行编译。

#### 4. 代码混淆 {#4-代码混淆}

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

#### 5.重要配置选项 {#5重要配置选项}

**\(1\) 采集H5页面数据**

如果您在App内嵌入了WebView（包括X5内核），请确保您已经调用过下面的方法，来采集H5页面的数据：

```text
WebView.setWebChromeClient(WebChromeClient client);
```

**请在第一次调用 WebView.loadUrl\(\)** 之前调用以上方法。

**\(2\) 采集Banner数据**

很多应用的界面上方都有横向滚动的 Banner 广告。

对于此类广告，如果您的应用通过 ViewPager、AdapterView 或者 RecyclerView 实现，请在 Banner创建时（包括动态创建）调用下面的接口来采集数据。

```text
GrowingIO.getInstance().trackBanner(banner, bannerDescriptions)
```

其中 bannerDescriptions 是 List&lt;String&gt;类型，包含所有广告图对应的广告内容描述，内容描述需要跟广告的顺序相同。

例如，当您有 5 张广告图时，只需创建一个 String 类型的 List，然后按 5 个广告出现的顺序给 List 的元素设置对应的广告描述，同样设置 5 个元素即可。

**\(3\) 采集GPS数据**

如果您需要采集用户的GPS数据，请在获取坐标后，调用如下接口进行设置

```text
GrowingIO.getInstance().setGeoLocation(latitude, longitude);
```

其中，`latitude`是纬度，`longitude`是经度。

当用户下一次切换页面，或者发生点击行为时，GPS数据会被发送回GrowingIO。

如果您需要清除用户的GPS信息，请调用如下接口

```text
GrowingIO.getInstance().clearGeoLocation();
```

**\(4\) 采集输入框数据**

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置

```text
GrowingIO.getInstance().trackEditText(EditText);
```

其中，`EditText`代表要被采集的输入框。

当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。

请注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。

**\(5\) 启用Hashtag识别**

在 SDK 初始化方法中设置

```text
    GrowingIO.startWithConfiguration(this, new Configuration().setHashTagEnable(true)）
```

**\(6\) 多进程支持**

SDK默认不支持多进程使用， 但是可以通过confiuration 进行设置支持多进程。 设置方法为， 在需要使用SDK功能的进程的Application onCreate中初始化SDK：

```text
GrowingIO.startWithConfiguration(this, new Configuration()
                  .supportMultiProcessCircle(true)
                  .setMutiprocess(true)));
```

其中supportMultiProcessCircle 与 setMutiprocess要同时使用， 而且多个进程中设置的值要相同。

**为什么不默认支持多进程？**

跨进程通信是一个相对较慢的过程， 默认不开启， 可以满足大部分用户的要求。

**哪些进程需要初始化SDK？**

需要使用SDK功能的进程需要初始化SDK， 所有的UI进程 + 部分Service进程\(如果这些进程中涉及手动打点\)。

### Android SDK API {#android-sdk-api}

#### API 简介 {#api-简介}

```text
// clearUserId API调用示例
GrowingIO.getInstance().clearUserId();
```

```text
// clearUserId API原型
GrowingIO.getInstance().clearUserId();
```

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

#### clearUserId {#clearuserid}

注：如果您的应用是App，且每次用户升级App版本时无需重新登录的话，建议在用户每次升级App版本后初次访问时重新调用上述 setUserId 方法。

```text
// setuserId API调用示例
GrowingIO.getInstance().setUserId(String "1234567890");
```

```text
// setUserId API原型
GrowingIO.getInstance().setUserId(String userId);
```

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- |
| userId | String | 是 | 用户的登录用户ID |

当用户登录之后调用setUserId API，设置登录用户ID。

#### setUserId {#setuserid}

```text
// people.set API调用示例二
GrowingIO gio = GrowingIO.getInstance();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setPeopleVariable(jsonObject);
```

```text
// people.set API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable("gender", "male");
```

```text
// people.set API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);
```

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 用户变量用于用户信息相关的分析 |

参数：

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

#### setPeopleVariable {#setpeoplevariable}

```text
// setEvar API调用示例二
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("campaignId", "1234567890");
jsonObject.put("campaignOwner", "Li Si");
gio.setEvar(jsonObject);
```

```text
// setEvar API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar("campaignId", "1234567890");
```

```text
// setEvar API原型
GrowingIO gio = GrowingIO.getInstance();
gio.setEvar(String key, String value);
gio.setEvar(String key, Number value);
gio.setEvar(String key, Boolean value);
gio.setEvar(JSONObject conversionVariables);
```

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 转化变量用于高级归因分析 |

参数

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

#### setEvar {#setevar}

```text
// page.set API调用示例
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setPageVariable(myActivity, jsonObject);
```

```text
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

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 页面级别的信息 |

参数：

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

#### setPageVariable {#setpagevariable}

```text
// track API调用示例三
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.track("loanAmount", 80000, jsonObject);
```

```text
// track API调用示例二
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.track("registerSuccess", jsonObject);
```

```text
// track API调用示例一
GrowingIO gio = GrowingIO.getInstance();
gio.track("registerSuccess");
```

```text
// track API原型
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);
```

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariable | JSON Object | 否 | 事件发生时所伴随的维度信息。 |

发送一个事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

#### track {#track}

```text
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

### 自定义数据上传&配置指导 {#自定义数据上传配置指导}

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，具体请参考[相关文档](../data-model/zi-ding-yi-shi-jian-he-bian-liang/custom-data-implement-guide.md)。

### 1.x 版本 SDK 升级指导 {#1x-版本-sdk-升级指导}

本文旨在帮助您从 1.x 版本无缝升级至 2.x 版本，由于两个版本的诸多接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

您需要完成以下几个步骤：

请您参考以下开发文档，完成SDK初始化代码的添加。

* [Android SDK 开发文档](android-sdk.md#sdk-安装)

Tips：建议您在开发中，使用 debug mode 校验 GrowingIO SDK 的数据是否正常上传。开启 debug mode 的方式请见上述文档中初始化方法部分。

**2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“[用户变量](../data-model/zi-ding-yi-shi-jian-he-bian-liang/custom-event-variable.md#用户变量)”。两者的区别主要在于：用户变量支持自定义的归因方式。

**2.1 上传接口：**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

* 1.x 版本方法格式：

```text
GrowingIO growingIO = GrowingIO.getInstance();
    growingIO.setCS1("user_id", "100324");
    growingIO.setCS2("company_id", "943123");
    growingIO.setCS3("user_name", "张溪梦");
```

* 2.x 版本方法格式：

对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```text
// 设置登录用户ID API
GrowingIO.getInstance().setUserId(String userId);

// 清除登录用户ID API
GrowingIO.getInstance().clearUserId();
```

对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```text
GrowingIO.setAppVariable(JSONObject variables)
GrowingIO.setAppVariable(String key, Number variable)
GrowingIO.setAppVariable(String key, String variable)
GrowingIO.setAppVariable(String key, Boolean variable)
```

对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```text
GrowingIO gio = GrowingIO.getInstance();
gio.setPeopleVariable(String key, String value);
gio.setPeopleVariable(String key, Number value);
gio.setPeopleVariable(String key, Boolean value);
gio.setPeopleVariable(JSONObject peopleVariables);// 多个变量，可组合为一个JSON对象peopleVariables传入

#### 2.2 GrowingIO 后台配置

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档](../sdk-1.x-docs/custom-data-config.md)。

#### 3. 迁移页面属性字段（PS字段）

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了[“页面级变量”](../../implementation/event-variable/custom-event/custom-variables-introduction/page-level-variable.md)。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

**3.1 上传接口**

* 1.x 版本方法格式：

```text
GrowingIO.setPageGroup(Activity activity, String name);
GrowingIO.setPS1(Activity activity, String property);
GrowingIO.setPS2(Activity activity, String property);
```

* 2.x 版本方法格式：

```text
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

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-model/zi-ding-yi-shi-jian-he-bian-liang/custom-data-implement-guide.md#自定义数据-上传步骤)

#### 4. 迁移自定义事件（埋点事件） {#4-迁移自定义事件埋点事件}

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

**4.1 上传接口**

```text
public class GrowingIO {
    public GrowingIO track(String eventName, JSONObject properties)
}
```

* 2.x 版本方法格式：

```text
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);
gio.track(String eventId, JSONObject eventLevelVariables);
```

**4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在\*\*“管理” - “自定义事件和变量” 页面中的 “自定义事件” Tab 页\*\*。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

#### 5. 数据校验 {#5-数据校验}

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。 对移动端的开发者，GrowingIO 的 SDK 提供了 debug 模式，在 SDK 初始化代码中可以找到。如下图：

![](https://docs.growingio.com/.gitbook/assets/12%20%281%29.jpeg)

开启 debug 模式后，您需要在App上触发一下打点事件，在打出的log里搜索上述关键字就能找到对应自定义事件&变量上传的数据。

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保您的自定义事件或变量确实有被触发。

### GrowingIO Mobile Debugger

GrowingIO Debugger是GrowingIO推出的调试 SDK所发送数据的工具。在GrowingIO Debugger的帮助下，实施工程师可以看到在什么样的页面上，在什么时机向GrowingIO发送了什么样的服务器请求。

在数据驱动增长的实践中，数据准确性一直以来是一个必须重视的课题。数据分析行业中有一句谚语：Garbage In, Garbage Out，简称GIGO。这句谚语告诉我们：如果我们在实施阶段的方案有纰漏导致收集的数据就是错误的，那么基于错误的数据得出的结论就一定是有问题的，所以我们必须重视GrowingIO产品的实施工作，来确保实施方案的严谨正确，从而保证收集到的数据是尽最大可能准确的。

而在GrowingIO产品的实施过程中，GrowingIO Debugger能够帮助工程师、技术实施顾问清楚的看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了GrowingIO Web Debugger的帮助，实施工程师可以通过高质量的实施，给分析师带来高质量高准确性的数据，这将大大有利于在企业内部养成数据驱动决策的文化和习惯。使用数据，使用高质量的数据帮助企业获得真正的增长。

[点击查看 GrowingIO Mobile Debugger 的安装和使用](growingio-debugger.md#growingio-mobile-debugger)。  


