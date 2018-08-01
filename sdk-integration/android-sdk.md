# Android SDK

* [集成](android-sdk.md#ji-cheng)
  * [添加依赖](android-sdk.md#1-tian-jia-yi-lai)
  * [添加 URLScheme 和网络权限](android-sdk.md#2-tian-jia-urlscheme-he-wang-luo-quan-xian)
  * [初始化SDK](android-sdk.md#3-chu-shi-hua-sdk)
  * [代码混淆](android-sdk.md#4-dai-ma-hun-xiao)
* [重要配置](android-sdk.md#zhong-yao-pei-zhi)
* [自定义事件和变量 API](android-sdk.md#zi-ding-yi-shi-jian-he-bian-liang-api)
* [验证SDK是否正常工作](android-sdk.md#yan-zheng-sdk-shi-fou-zheng-chang-gong-zuo)
* [附录](android-sdk.md#fu-lu)
* [旧版本升级](android-sdk.md#jiu-ban-ben-sheng-ji)
  * [更新 SDK 版本号为最新版本](android-sdk.md#1-geng-xin-sdk-ban-ben-hao-wei-zui-xin-ban-ben)
  * [迁移用户属性字段（CS字段）](android-sdk.md#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [迁移页面属性字段（PS字段）](android-sdk.md#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [迁移自定义事件（埋点事件）](android-sdk.md#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [数据校验](android-sdk.md#5-shu-ju-xiao-yan)
* [旧版本 SDK 集成 ](android-sdk.md#jiu-ban-ben-sdk-ji-cheng)
* [常见问题](android-sdk.md#chang-jian-wen-ti)

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
        classpath 'com.growingio.android:vds-gradle-plugin:2.4.1'
    }
}
```

**\(2\)在module级别的build.gradle文件中添加com.growingio.android插件、vds-android-agent依赖和对应的资源；**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种：

* 新产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt; 第二段中"此应用的 URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
* 现有产品的 URL Scheme ：登录官网 -&gt;点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![](../.gitbook/assets/image%20%289%29.png)

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
        compile 'com.growingio.android:vds-android-agent:2.4.1@aar'
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

## 重要配置

#### **配置项列表**

| API | 说明 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| setDebugMode | 查看数据采集发送日志 |
| setTestMode | 实时发送数据，开启则不遵循移动网络状态下数据发送大小限制以及缓存策略 |
| trackBanner | 采集`Banner`数据，添加对`Banner`图片的描述 |
| setGeoLocation | 采集用户`GPS`数据 |
| trackEditText | 采集输入框内容 |
| setMultiprocess | 支持多进程数据采集 |
| supportMultiprocessCircle | 支持多进程圈选 |
| setHashTagEnable | 支持 |
| ~~setWebChromeClient~~ | ~~确保采集H5页面的数据，在第一次调用 WebView.loadUrl\(\) 之前调用此方法~~ （`SDK 2.3.1` 之后不用配置，请使用最新版 SDK） |

\*\*\*\*

### **设置 Debug 模式**

#### **setDebugMode**

查看数据采集发送日志，能够在`Android Studio`中通过`Logcat`查看`GrowingIO`打印的数据发送日志，在 `APP` 的 `Application onCreate` 初始化`SDK`地方添加配置。

```java
setDebugMode(boolean debugMode);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- |
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
| --- | --- |
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

### **采集广告 Banner 数据**

#### **trackBanner**

采集`Banner`数据，应用的界面上方横向滚动的`Banner`广告。

```java
GrowingIO.getInstance().trackBanner(View banner,List<String> bannerDescriptions);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- | --- |
| banner | View |   是           |    无                 | ViewPager、AdapterView、RecyclerView 实现的 View |
| bannerDescriptions | List&lt;String&gt; |   是 |    无 | 广告内容描述，顺序需要跟 banner view 顺序一致 |

**示例代码：**

```java
 //Banner 初始化代码
 ViewPager banner = (ViewPager)findViewById(R.id.banner);
 ...
 //GrowingIO 采集 Banner 数据
 GrowingIO.trackBanner(banner.getViewPager(), Arrays.asList("banner 1", "banner 2", "banner 3"));
```

{% hint style="warning" %}
`Banner`描述和广告出现的顺序一致，调用通过`Log`查看或者[`MobileDebugger`](growingio-debugger/#shi-yong-mobile-debugger-ce-shi-shu-ju)的方式检查配置是否正确。 查看 `Banner imp` 事件 `e` 字段中 `v` 是否为您设置的 banner description。
{% endhint %}

**检验数据发送日志示例：** 

```javascript
{
    "s":"02015456-079b-49cc-8b42-a5cc6136f0ec",
    // t 为事件类型 type
    "t":"imp", 
    "tm":1531990474178,
    "d":"com.growingio.android.test",
    "p":"ConvenientBannerActivity",
    "ptm":1531990413207,
    "e":[
        {
            "x":"/MainWindow/LinearLayout[0]/FrameLayout[1]/FitWindowsLinearLayout[0]#action_bar_root/ContentFrameLayout[1]/FrameLayout[0]/RelativeLayout[0]/ConvenientBanner[0]#convenientBanner/LinearLayout[0]/RelativeLayout[0]/CBLoopViewPager[0]#cbLoopViewPager/ImageView[-]",
            "tm":1531990474179,
            "idx":1,
            //假如现在滚动到第二个banner，V显示为您设置的描述
            "v":"banner 2" 
        }
    ]
}
```



### 采集 GPS 数据

#### setGeoLocation

精确采集`GPS`数据，请在获取坐标后，调用接口设置位置信息。当用户下一次切换页面，或者发生点击行为时，`GPS`数据会被发送回`GrowingIO`。如果您不调用此接口也可以，我们会根据用户的`ip`定义用户位置，能够在最终的数据分析时看到`APP`用户地域分布。

```java
GrowingIO.getInstance().setGeoLocation(double latitude,double longitude);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- | --- |
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

\*\*\*\*

### **采集输入框数据**

#### **trackEditText**

`GrowingIO` 默认采集输入框点击次数，不采集文字，在您需要采集应用内某个输入框内的文字（例如搜索框）时使用。当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回`GrowingIO`。

```java
GrowingIO.getInstance().trackEditText(EditText editText);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- |
| editText | EditText |   是 |   无 | 采集输入框 |

**示例代码：**

```java
EditText editText = (EditText) findViewById(R.id.edit_text);
GrowingIO.getInstance().trackEditText(editText);
```

{% hint style="warning" %}
1. 对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。
2. 在输入框失去焦点的时候或者`onPause`时，才会采集事件，如果输入完成，输入框光标仍然在闪动，则不会采集事件。为了保证数据准确性，请每次用户输入完成时，让输入框失去焦点，可使用`editText.setFocusable(false);`
{% endhint %}



### WebView 锚点链接页面访问

#### setHashTagEnable

开启之后，APP `WebView` 中的页面，如果用户点击带有锚点的跳转链接，则发送一次页面浏览事件，请在 `SDK` 初始化方法中设置，默认值为 false，即默认锚点链接不算做页面浏览事件。

GrowingIO 默认不会把`HashTag`识别成页面 URL 的一部分，对于使用`HashTag`进行页面跳转的单页面网站应用来说，可以启用它来标识页面，`HashTag`的值也会记录在页面URL中。

```java
GrowingIO.startWithConfiguration(this, new Configuration().setHashTagEnable(true));
```

**例如：**

点击 APP `WebView` 中代码混淆的锚点链接，URL 中`#`号后面为锚点，设置后 SDK 会发送页面浏览事件，它的链接为：

> [https://docs.growingio.com/docs/sdk-integration/android-sdk\#4-dai-ma-hun-xiao](https://docs.growingio.com/docs/sdk-integration/android-sdk#4-dai-ma-hun-xiao)



![&#x70B9;&#x51FB;WebView&#x4E2D;&#x7684;&#x951A;&#x70B9;&#x94FE;&#x63A5;](../.gitbook/assets/image%20%2859%29.png)

SDK发送对应采集数据：

```javascript
{
    "u":"2e94075c-ead2-33ac-ab7f-f9611162efc4",
    "s":"78383569-3038-4bd4-b27c-349939367922",
    "tl":"Android SDK - 帮助文档",
    //t 为事件类型，page 为页面浏览事件
    "t":"page",
    "tm":1532408777047,
    "pt":"https",
    "d":"com.growingio.android.test::docs.growingio.com",
    //p 为页面，页面为 “/docs/sdk-integration/android-sdk#4-dai-ma-hun-xiao”
    "p":"StandardWebView::/docs/sdk-integration/android-sdk#4-dai-ma-hun-xiao",
    "rf":"https://docs.growingio.com/docs/sdk-integration/android-sdk#ji-cheng",
    "cs1":"GrowingIO",
    "appId":"fakeAppID",
    "v":"Android SDK - 帮助文档",
    "r":"WIFI",
    "gesid":434,
    "esid":153
}
```



### 多进程支持

#### setMutiprocess & supportMultiProcessCircle

多进程支持，`SDK`默认不支持多进程使用， 但是可以通过`confiuration`进行设置支持多进程。 在`Application onCreate` 中初始化SDK代码块中配置。

```java
//支持多进程数据采集
setMutiprocess(boolean setMutiprocess);
//支持多进程圈选
supportMultiProcessCircle(boolean smpc);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- | --- |
| isMultiprocess | boolean |   是 |  false | 开启多进程数据采集 |
| smpc | boolean |   是 |  false | 开启多进程圈选 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                  .supportMultiProcessCircle(true)
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

**`SDK 2.3.2`** 以上版本支持

| 接口名称 | 含义 |
| --- | --- | --- |
| disableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户不授权，不采集用户数据 |
| enableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户授权，采集用户数据 |



### DeepLink 回调参数获取

#### setDeeplinkCallback  （`SDK 2.3.2`以上版本支持）

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
| --- | --- | --- |
| params                | Map&lt;String,String&gt; | 自定义参数，您自定义的键值对 |
| status | int | DeeplinkCallback.SUCCESS ：自定义参数获取成功； DeeplinkCallback.PARSE\_ERROR ：解析异常；DeeplinkCallback.ILLEGAL\_URI ：非法URI； DeeplinkCallback.NO\_QUERY : 自定义参数为空。 |

**示例代码：**

```java
GrowingIO.startWithConfiguration(this, new Configuration()
                .setDeeplinkCallback(new DeeplinkCallback() {
                            @Override
                            public void onReceive(Map<String, String> params, int error) {
                                 if (status == DeeplinkCallback.SUCCESS) {
                                    //获得您的自定义参数，处理您的相关逻辑
                                    Log.d("TestApplication", "DeepLink 参数获取成功，params" + params.toString());
                                }
                            }
                        })
            );
```



### ~~采集 WebView 页面数据~~

####  ~~setWebChromeClient~~

#### ~~采集H5页面数据~~**（SDK 2.3.1 之后不用配置，请使用最新版）**

~~如果您在App内嵌入了WebView（包括X5内核），请确保您已经调用过下面的方法，来采集H5页面的数据：~~

```java
WebView.setWebChromeClient(WebChromeClient client);
```

~~**请在第一次调用 WebView.loadUrl\(\)** 之前调用以上方法。~~

{% hint style="info" %}
目前仅支持 Android 原生`WebView`和腾讯`X5WebView`的无埋点数据采集，其中淘宝、有赞、APPCan、UC 默认不支持无埋点数据采集，如有需求请联系技术支持。
{% endhint %}

### 

## 自定义事件和变量 API

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，进行[数据分析](../data-analytics/)。除自动收集的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](../data-defination/events-metrics/manual-metrics.md)和[变量](../data-defination/dimensions/manual-dimensions.md)，下面介绍自定义事件和变量 API 使用方法，后文简称埋点事件API。



### API 简介 

```java
// 发送自定义事件 API
GrowingIO gio = GrowingIO.getInstance();
gio.track(String eventId);
gio.track(String eventId, Number eventNumber);
gio.track(String eventId, JSONObject eventLevelVariables);
gio.track(String eventId, Number eventNumber, JSONObject eventLevelVariables);

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

| 参数名称 | 参数类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加一；当出现number参数时，事件自增number的数值 |
| eventLevelVariable | JSONObject | 否 | 事件发生时所伴随的维度信息 |

**参数限制条件：**

参数违反以下条件将不发送数据，`SDK 2.4.0` 以上能够在 Log 日志中查看对应报错，之下版本无提示信息。调用后请关注日志，查看数据发送是否成功，事件类型`t`为`cstm`。

| 参数名称 | 限制条件 |
| --- | --- | --- | --- |
| eventId | 非空，长度限制小于等于50；`SDK 2.4.0`以下版本不支持中文，仅支持 0 到 9、a 到 z 以及下划线，并且不能以数字开头。 |
|  number | 非空。 |
| eventLevelVariable | 非空，长度限制小于等于100（`eventLevelVariable.length()<=100`）；`eventLevelVariable` 内部不允许含有`JSONObject`或者`JSONArray；key` 长度限制小于等于50，`value` 长度限制小等于1000。 |

**示例代码：**

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
#### 推荐您使用MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[cstm 事件验证](growingio-debugger/best-practice.md#cstm-shi-jian-yi-ji-guan-lian-de-shi-jian-ji-bian-liang-shi-jian)。
{% endhint %}

### 

### setPageVariable

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

使用此接口时，请先阅读 [**GrowingIO 对于页面的定义**](android-sdk.md#1growingio-dui-yu-ye-mian-de-ding-yi)。

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

{% hint style="info" %}
请注意这里的`activity`和`fragment`参数，[**它关乎GrowingIO对于页面的定义**](android-sdk.md#1growingio-dui-yu-ye-mian-de-ding-yi)。

参数选择当前界面上最后一个初始化完成的对象引用，例如一个`Activity`中嵌套多个`Fragment`情况，当前页面最后初始化完成的是`Fragment`，请确认当前页面`Fragment`，并且得到其当前引用，`new`对象将不会发送的哦。
{% endhint %}

**参数说明：**

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- | --- | --- |
| activity | Activity | 是 | **最后一个初始化完成的页面** |
| fragment | Fragment | 是 | **最后一个初始化完成的页面** |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSONObject | 否 | 页面级别的信息 |

**参数限制条件：**

参数违反以下条件将不发送数据，`SDK 2.4.0` 以上能够在 Log 日志中查看对应报错，之下版本无提示信息。调用后请关注日志，查看数据发送是否成功，事件类型`t`为`pvar`。

| 参数名称 | 限制条件 |
| --- | --- | --- | --- |
| key  | 非空，长度限制小于等于50。 |
| value | 非空，长度限制小于等于1000。 |
| pageLevelVariables | 非空，长度限制小于等于100（`pageLevelVariable.length()<=100`）；`pageLevelVariable` 内部不允许含有`JSONObject`或者`JSONArray`；`key` 长度限制小于等于50，`value`长度限制小等于1000。 |

**示例代码：**

```java
// page.set API调用示例
GrowingIO gio = GrowingIO.getInstance();
JSONObject jsonObject = new JSONObject();
jsonObject.put("gender", "male");
jsonObject.put("age", "21");
gio.setPageVariable(myActivity, jsonObject);
```

**检验数据发送日志示例：** 

注意 `t` 等于`pvar`字段，表示自定义事件发送成功，只需注意 `var`字段，其它字段无需仔细验证**。**

```javascript
{
    "s":"0a7e8150-c409-45d4-96ed-b5781fe652cb",
    // t 为事件类型，pvar 页面级事件
    "t":"pvar",
    "tm":1532337448255,
    "d":"com.growingio.android.test",
    "cs1":"GrowingIO",
    "p":"SetUserIdFragment1",
    "ptm":1532337448255,
    //页面级变量
    "var":{
        "gender":"male",
        "age":"21"
    },
    "gesid":292,
    "esid":0
}
```

{% hint style="danger" %}
注意确认当前页面，通过[圈选](android-sdk.md#yu-bei-zhi-shi)方式最快能够定位当前页面，在当前页面埋点最稳定可靠。如果页面未确认，可能在`Activity`和`Fragment`嵌套的场景下埋点失败。
{% endhint %}

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ pvar 事件验证](growingio-debugger/best-practice.md#pvar-ye-mian-ji-bian-liang-shi-jian)
{% endhint %}

### 

### setEvar

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
| --- | --- | --- | --- |
| key | String | 否 | 转化变量的标识符 |
| value | String | 否 | 转化变量的值 |
| conversionVariables | JSONObject | 否 | 转化变量用于高级归因分析 |

**参数限制条件：**

| 参数名称 | 限制条件 |
| --- | --- | --- | --- |
| key | 非空，长度限制小于等于50。 |
| value | 非空，长度限制小于等于1000。 |
| conversionVariables | 非空，长度限制小于等于100（`conversionVariables.length()<=100`）；`conversionVariables` 内部不允许含有`JSONObject`或者`JSONArray`；`key` 长度限制小于等于50，`value`长度限制小等于1000。 |

**示例代码：**

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
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ evar 事件验证](growingio-debugger/best-practice.md#evar-zhuan-hua-bian-liang-shi-jian)
{% endhint %}

### 

### setPeopleVariable

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

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
| --- | --- | --- | --- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| peopleVariables | JSONObject | 否 | 用户变量用于用户信息相关的分析 |

**参数限制条件：**

| 参数名称 | 限制条件 |
| --- | --- | --- | --- |
| key | 非空，长度限制小于等于50。 |
| value | 非空，长度限制小于等于1000。 |
| peopleVariables | 非空，长度限制小于等于100（`peopleVariables.length()<=100`）；`peopleVariables` 内部不允许含有`JSONObject`或者`JSONArray`；`key`长度限制小于等于50，`value`长度限制小等于1000。 |

**示例代码：**

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
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ ppl 事件验证](growingio-debugger/best-practice.md#ppl-yong-hu-bian-liang-shi-jian)
{% endhint %}

### 

### setUserId

当用户登录之后调用setUserId API，设置登录用户ID。

```java
GrowingIO.getInstance().setUserId(String userId);
```

**参数说明：**

| 参数名称 | 参数类型 | 必填 | 说明 |
| --- | --- |
| userId | String | 是 | 登录用户Id，长度限制小于等于1000 |

**示例代码：**

```java
GrowingIO.getInstance().setUserId("1234567890");
```

注：您的 App 每次用户升级版本时无需重新登录的话，建议在用户每次升级App 版本后初次访问时重新调用上述 setUserId 方法。

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ 用户变量](growingio-debugger/best-practice.md#chang-jing-yi-yong-hu-bian-liang-zhi-deng-lu-yong-hu-id)
{% endhint %}

### 

### clearUserId

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

**示例代码：**

```java
GrowingIO.getInstance().clearUserId();
```



### setVisitor

当用户未登录时，定义用户属性变量，也可用于A/B测试上传标签。

```java
GrowingIO.getInstance().setVisitor(JSONObject visitorVar)
```

**参数说明：**

| 参数名称 | 参数类型 | 必填 | 说明 |
| --- | --- |
| visitorVariable | String | 是 |  |

**示例代码：**

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

\*\*\*\*

\*\*\*\*

## 验证 SDK 是否正常工作

验证 SDK 的采集事件数据发送至关重要，数据采集是数据分析的基础，保证了数据采集的精准才能帮助您做出正确的数据分析。GrowingIO 一直在努力满足市面上所有 APP 的数据自动采集，也成为无埋点数据采集，但是因为开发者的实现方式在江湖上不统一， 我们覆盖事件采集触发逻辑可能与您的实现方式有出入，所以强烈建议开发者验证 GrowingIO SDK 的数据发送，如果有任何问题，及时反馈技术支持，我们会一直努力增强我们的采集能力，给您更好的服务。

### GrowingIO采集事件介绍

GrowingIO 的数据采集分为自动采集和用户自定义事件和变量两种方式采集移动端APP，也就是通常所说的无埋点和埋点。下面将分别介绍这两种方式GrowingIO定义的采集事件类型。

#### 自动采集事件类型

| 事件类型 | 含义 | 发送时机 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| activate | 激活 | 当APP首次激活打开时 |
| vst | 应用访问 | 1.冷启动发送2.切换用户发送3.默认APP进入后台30秒以后再次打开会发送 |
| page | 页面浏览 | 每当进入一个页面时发送 |
| imp | 元素展现 | 页面元素发生变动的时候触发，比如弹框或者`ListView`滑动 |
| clck | 点击 | 点击实现了相关 click Listener 的元素控件，[常见问题中将详细列举](android-sdk.md#2-dian-ji-shi-jian-cai-ji-luo-ji) |
| chng | 输入框内容变化 | 输入框`EditText`失去焦点，默认不采集输入内容 |
| reengage | DeepLink唤醒事件 | 通过扫描 DeepLink 二维码唤醒 APP 后发送 |

#### 自定义事件类型

| 事件类型 | 含义 | 发送时机 |
| --- | --- | --- | --- | --- | --- |
| cstm | 自定义事件触发 | 调用[`track`](android-sdk.md#track)后发送 |
| pvar | 页面级变量触发 | 调用[`setPageVariable`](android-sdk.md#setpagevariable)后发送 |
| evar | 转化变量触发 | 调用[`setEvar`](android-sdk.md#setevar)后发送 |
| ppl | 设置用户变量 | 调用[`setPeopleVariable`](android-sdk.md#setpeoplevariable)后发送 |
| vstr | 设置访问用户变量 | 调用[`setVisitor`](android-sdk.md#setvisitor)后发送 |



### 验证工具

#### [1. MobileDebugger](growingio-debugger/#qi-dong-mobile-debugger)

#### [2.查看日志](android-sdk.md#setdebugmode)



### 圈选功能验证

请先依据[圈选文档](../data-defination/events-metrics/circle-metrics/app-circle.md)，唤醒圈选功能，主要检查元素是否都可圈选，重点尝试WebView内部元素是否可圈。

当显示高亮则证明可圈，如图所示：

![&#x5143;&#x7D20;&#x9AD8;&#x4EAE;&#x8BC1;&#x660E;&#x53EF;&#x5708;](../.gitbook/assets/image%20%2869%29.png)

\*\*\*\*



## 附录

GrowingIO 提供了**初始化配置项 API** 和**运行时 API** 来自定义SDK的采集，满足不同场景的定制采集。

### GrowingIO 初始化配置项 API 

GrowingIO 配置项均在`Application`的`onCreate`方法中SDK初始化代码块中设置，全部可配置项如下，下面将分类并描述含义。

```java
GrowingIO.startWithConfiguration(this,
new Configuration()
    .disableCellularImp()
    .disableImageViewCollection(false)
    .setBulkSize(100)
    .setCellularDataLimit(1000)
    .setChannel("渠道号")
    .setDebugMode(true)
    .setDeeplinkCallback(new DeeplinkCallback() {
        @Override
        public void onReceive(Map<String, String> params, int error) {

        }
    })
    .setDiagnose(false)
    .setDisabled(false)
    .setDisableImpression(false)
    .setFlushInterval(1000)
    .setMutiprocess(true)
    .setSampling(0.34)
    .setSessionInterval(23000)
    .setTestMode(true)
    .setThrottle(false)
    .setTrackWebView(true)
    .supportMultiProcessCircle(true)
    .trackAllFragments()
);
```



#### 基础配置 API

| 接口名称 | 默认值 | 含义 |
| --- | --- | --- | --- |
| setChannel\(`String` channel\) | 无             | 设置渠道 |
| ~~useID\(\)~~ | `true` | 已经废弃 @deprecated是否在计算`xpath`的时候控件使用`id，`默认使用`id`更精准，在版本`SDK 2.5.0`将删除此接口 |
| setDeeplinkCallback\(`DeeplinkCallback`callback\) | 无 | DeepLink 回调接口，获得自定义参数以便跳转对应 APP页 面 |

#### 功能 API

| 接口名称 | 默认值 | 含义 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| setDisabled\(`boolean` disabled\) | `false` | 设置为`true`时不采集数据 |
| setTrackWebView\(`boolean` trackWebView\) | `true` | 设置为`false`时不采`WebView`数据 |
| supportMultiProcessCircle\(`boolean` spmc\) | `false` | 是否使用了多进程圈选 |
| setMutiprocess\(`boolean` isMultiprocess\) | `false` | 是否使用了多进程 |
| trackAllFragments\(\) | `false` | 是否采集所有Fragment；调用则设置为`true` |
| disableCellularImp\(\) | `false` | 否关闭移动蜂窝网`impression`数据采集 |
| setHashTagEnable\(`boolean` hashTagEnable\) | `false` | 是否认为点击锚点链接的跳转是一个页面浏览 |

#### 数据采集相关 API

| 接口名称 | 默认值 | 含义 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| setSampling\(`double` sampling\) | 1 | 采样率\[0.01~1\],若设置sampling = 0.01，则 1% 的设备会被采集数据，每次启动会根据用户设置的采样率判断设备是否在采集的范围之内，使用**之前请咨询技术支持** |
| setSessionInterval\(`long` sessionInterval\) | 30 \* 1000 | 在后台停留时长超过此值，则产生新的`sessionId`,发送`visit`事件。 |
| setThrottle\(`boolean` throttle\) | `false` | 是否节流发送（节流发送时`imp`不发送），不发送但是采集 |
| setFlushInterval\(`long`  flushInterval\) | 30 \* 1000 | 数据刷新的最长时间间隔，默认30 秒 。如果距离上次发送数据事件超过此时间则发送事件 |
| setDisableImpression\(`boolean`disableImp\) | `true` | 是否采集`imp`事件 |
| setCellularDataLimit\(`long` cellularDataLimit\) | 3 \* 1024 \* 1024 | 一天的时间之内，在移动蜂窝网下的数据最大传输量，默认3M。 |
| setBulkSize\(`int` bulkSize\) | 300 | 如果数据库存储数据条数大于等于`bulkSize`，则马上发送数据。 |

### 

### GrowingIO 运行时 API 

GrowingIO 为 APP 提供运行时随意调用的 API ，使用方法如下：

```java
// 得到 GrowingIO 实例后可以调用其中 API
GrowingIO gio = GrowingIO.getInstance();
gio.setUserId("张溪梦");
```

{% hint style="danger" %}
GrowingIO 所有 API 都需要在主线程调用。
{% endhint %}

\*\*\*\*

**API 明细：**

| 接口名称 | 含义 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [trackBanner\(`View` banner, `List<String>` bannerContents\)](android-sdk.md#trackbanner) | [因为 SDK 不能识别 Banner ,配置这个接口则不监听 banner 中的 Fragment 的生命周期，不采集生命周期涉及的用户行为数据](android-sdk.md#trackbanner) |
| ignoredView\(`View` view\) | 忽略配置的 View ，不采集用户数据 |
| ignoreFragment\(`Activity` activity, `Fragment` fragment\) | 忽略配置的 View ，不采集用户数据 |
| setViewInfo\(`View` view, `String` info\) | 配置 view 的 Tag，标记 View ，并在 GrowingIO 相关事件中发送 ，内容对应 `xPath` 中的 `obj` 例如：在商品`ListView`添加购物车的场景中，每个商品`item`都含有一个加入购物车按钮，这时无法区分商品`item`与将它加入购物车按钮的一对一关系，此时调用此方法增加描述。注意：适用于原有`v`字段含义不大，只关注描述的场景，使用此接口后`v`字段将不采集 |
| setViewContent\(`View` view, `String` content\) | 配置 view 的 Tag，标记 View ，并在 GrowingIO相关事件中发送，内容对应 `xPath` 中的 `v` SDK默认不会采集ImageView的内容，为了能对不同的图片元素（ImageView）区分统计，需要对每个具有分析意义的图片元素（ImageView）添加描述。 |
| setPageName\(`Activity` activity, `String` name\) | 设置页面别名 |
| setPageName\(`Fragment` fragment, `String` name\) | 设置页面别名 |
| setViewID\(`View` view, `String` id\) | 设置 View id ，配置之后对应 xPath 中的 view id当您的应用界面改版时，可能会导致无法准确地统计已经圈选的元素。因此，对于应用中的主要流程涉及到的界面元素，建议您为它们设置固定的唯一ID，以保证数据的一致性。 |
| setThrottle\(boolean throttle\) | 是否节流发送（节流发送时imp不发送），内部实际调用 Configuration 中的同名方法，所以在初始化时候配置和运行时动态配置，效果一样。 |
| disable\(\) | GrowingIO 停止采集 |
| resume\(\) | GrowingIO 恢复采集 |
| stop\(\) | GrowingIO 停止采集，可以不在主线程调用 |
| getSessionId\(\) | 得到 session id |
| [trackEditText\(`EditText` editText\)](android-sdk.md#trackedittext) | [SDK 默认不采集用户输入框的内容，设置以后，采集除了密码以外的输入框文本内容。](android-sdk.md#trackedittext) |
| trackFragment\(`Activity` activity, `Fragment` fragment\) | 如果APP初始化时候，没有设置 `trackAllFragment` 即不采集全部 `Fragment`，可以选择性采集指定 `Fragment`，设置之后 sdk 将监听 `Fragment` 的各个生命周期， 采集相关用户行为数据。  |
| setGeoLocation\(`double` latitude, `double` longitude\) | 设置经纬度，并在 vst 事件中发送 |
| clearGeoLocation\(\) | 清空经纬度 |
| setChannel \(`String` channel\) | 设置渠道名称 |
| disableImpression\(\) | 不发送 `imp` |
| setImp\(`boolean` enable\) |  `imp`事件开关，`true` 为打开 |
| trackWebView\(`WebView` webview, `WebChromeClient` client\) | 采集 WebView 事件，默认采集，您可以在不全量采集WebView的时候，定制采集某个`WebView` |
| trackX5WebView\(`com.tencent.smtt.sdk.WebView` webView, `com.tencent.smtt.sdk.WebChromeClient` client\) | 采集 X5WebView 事件，默认采集 |





## 旧版本升级

{% hint style="danger" %}
#### 升级到 2.x SDK 前，请务必联系客服协助您完成后台配置的升级！
{% endhint %}

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

![](../.gitbook/assets/image%20%2867%29.png)

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



## 常见问题

### 1. GrowingIO 对于页面的定义

**Android 常见的应用场景是一个`Activity`中嵌套多个`Fragment`，那么我们是怎么定义页面的呢？**

APP进入一个页面之后，无论其中有多少层`Fragment`嵌套，200ms 内最后一个初始化完成的`Fragment`即认为当前的页面。在用户可见的界面上，有事件操作的归因都会这个`Fragment`上。您在埋点的时候一定要确认当前的页面，并在当前的页面埋点是最稳定可靠的。

确认当前页面方法有三种：

1.[圈选](../data-defination/events-metrics/circle-metrics/app-circle.md)，查看圈选页面为当前页面

![](../.gitbook/assets/image%20%2850%29.png)

2.[查看日志](android-sdk.md#setdebugmode)，进入页面发送的`page`的`p`为当前的页面

3.使用[`Mobile Debugger`](growingio-debugger/#shi-yong-mobile-debugger-ce-shi-shu-ju)查看`page`事件的`p`



### 2. 点击事件采集逻辑

设置以下点击事件的控件会被采集点击事件，如果您自定义了点击事件，不在下方列举之内，将无法采集点击事件，影响数据分析。

```java
onCheckedChanged(android/widget/CompoundButton)
onCheckedChanged(android/widget/RadioGroup)
onClick(android/content/DialogInterface)
onClick(android/view/View)
onItemClick(android/widget/AdapterView;android/view/View)
onItemSelected(android/widget/AdapterView;android/view/View)
onNewIntent(android/content/Intent)
onRatingChanged(android/widget/RatingBar)
onStopTrackingTouch(android/widget/SeekBar)
onFocusChange(android/view/View)
onMenuItemClick(android/view/MenuItem)
onOptionsItemSelected(android/view/MenuItem)
onGroupClick(android/widget/ExpandableListView;android/view/View)
onChildClick(android/widget/ExpandableListView;android/view/View)
```



### 3. SDK 编译时性能和消耗时间

GrowingIO Android SDK 的编译时耗时取决于您的项目大小，我们的原理是字节码插桩\(使用Transform API\)。  
从clean项目， 执行assembleDebug， 如果添加了GrowingIO的SDK， 会大约增加50%的时间， 如果执行assembleRelease， 添加GrowingIO SDK 大约会增加30%的时间。   
可以看出GrowingIO确实会影响您的编译时长，尤其是在项目比较大的情况。  
如果您感觉到明显的编译耗时长，我们提供了一个在开发期间 GrowingIO 不参与编译的配置，如下：

1.在 Project 项目中，gradle.properties 文件内添加

```text
# true GrowingIO 参与编译，false 不参与编译
gioenable = true
```

2.在 Module 级别的 build.gradle 文件中增加配置 

```groovy
android {
    defaultConfig {
        resValue("string", "growingio_project_id", "您的项目ID")
        resValue("string", "growingio_url_scheme", "您的URL Scheme")
        // 增加 gioenable 的配置
        resValue("string", "growingio_enable", project.gioenable)
    }
}
```

{% hint style="danger" %}
**上线时，一定要将 gradle.properties 文件中的 gioenable 改为 true 。否则我们将无法采集数据。**
{% endhint %}

