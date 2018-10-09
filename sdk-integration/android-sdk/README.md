# Android SDK

* [集成 SDK](./#ji-cheng-sdk)
  * [添加依赖](./#1-tian-jia-yi-lai)
  * [添加 URLScheme 和网络权限](./#2-tian-jia-urlscheme-he-wang-luo-quan-xian)
  * [初始化SDK](./#3-chu-shi-hua-sdk)
  * [代码混淆](./#4-dai-ma-hun-xiao)
* [重要配置](./#zhong-yao-pei-zhi)
* [自定义事件和变量 API](./#zi-ding-yi-shi-jian-he-bian-liang-api)
* [验证SDK是否正常工作](./#yan-zheng-sdk-shi-fou-zheng-chang-gong-zuo)
* [旧版本升级](./#jiu-ban-ben-sheng-ji)
  * [更新 SDK 版本号为最新版本](./#1-geng-xin-sdk-ban-ben-hao-wei-zui-xin-ban-ben)
  * [迁移用户属性字段（CS字段）](./#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [迁移页面属性字段（PS字段）](./#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [迁移自定义事件（埋点事件）](./#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [数据校验](./#5-shu-ju-xiao-yan)

## 集成 SDK

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
        classpath 'com.growingio.android:vds-gradle-plugin:2.4.4'
    }
}
```

**\(2\)在module级别的build.gradle文件中添加com.growingio.android插件、vds-android-agent依赖、项目ID和 URL Scheme**

URL Scheme的格式是growing.xxxxxxxxxxxxxxxx，它的获取方式有两种：

* 新产品的 URL Scheme ：登录官网 -&gt;点击“设置”icon -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt; 选择添加Android应用 -&gt;  URL Scheme 为:growing.xxxxxxxxxxxxxxxx”中标黄字体。
* 现有产品的 URL Scheme ：登录官网 -&gt; 点击“设置” icon -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme。

![&#x5E94;&#x7528;&#x7BA1;&#x7406;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2836%29.png)

您的项目ID获取方式是：点击“设置”icon-&gt;点击“项目配置”-&gt;即可看到您的项目ID

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
        compile 'com.growingio.android:vds-android-agent:2.4.4@aar'
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
| :--- | :--- |
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

### **采集广告 Banner 数据**

#### **trackBanner**

采集`Banner`数据，应用的界面上方横向滚动的`Banner`广告。对于此类广告，如果您的应用通过ViewPager、AdapterView或者RecyclerView实现，请在Banner创建时（包括动态创建）调用下面的接口来采集数据。

```java
GrowingIO.getInstance().trackBanner(View banner,List<String> bannerDescriptions);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
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
`Banner`描述和广告出现的顺序一致，调用通过`Log`查看或者[`MobileDebugger`](../growingio-debugger/#shi-yong-mobile-debugger-ce-shi-shu-ju)的方式检查配置是否正确。 查看 `Banner imp` 事件 `e` 字段中 `v` 是否为您设置的 banner description。
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

\*\*\*\*

### **采集输入框数据**

#### **trackEditText**

`GrowingIO` 默认采集输入框点击次数，不采集文字，在您需要采集应用内某个输入框内的文字（例如搜索框）时使用。

当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回`GrowingIO`。

```java
GrowingIO.getInstance().trackEditText(EditText editText);
```

**参数说明：**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
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



![&#x70B9;&#x51FB;WebView&#x4E2D;&#x7684;&#x951A;&#x70B9;&#x94FE;&#x63A5;](../../.gitbook/assets/image%20%28103%29.png)

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
    "rp":"https://docs.growingio.com/docs/sdk-integration/android-sdk#ji-cheng",
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
| :--- | :--- | :--- | :--- | :--- |
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
| :--- | :--- |
| disableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户不授权，不采集用户数据 |
| enableDataCollect\(\) | 遵守欧洲联盟出台的通用数据保护条例，用户授权，采集用户数据 |



### Deep Link 回调参数获取

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



### 自定义点击事件

如果您有自定义的控件重写了`View`的`onTouchEvent`方法来判断和处理点击事件，那么必须调用它的`PerformClick`，并且设置相应的`onClickListener`。

或者使用[手动调用系统点击事件方法](android-chang-jian-wen-ti.md#dian-ji-shi-jian-cai-ji-luo-ji)。



### 设置页面别名

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

{% hint style="info" %}
1. 必须在该`Activity`的`onCreate`方法中完成该属性的赋值操作。
2. 页面别名只能设置为字母、数字和下划线的组合。
3. 为查看数据方便，请尽量对iOS和安卓的同功能页面取不同的名称。
{% endhint %}



### 设置界面元素内容

SDK默认不会采集ImageView的内容，如果您需要区分不同的`ImageView`，可以使用contentDescription来标记，也可以调用下方的方法来设置：

```java
GrowingIO.setViewContent(View view, String contentDescription);
```

### 

### 设置界面元素ID

SDK会使用Layout文件中的ID来识别一个元素。

如果部分元素在Layout文件中没有ID，建议在Layout文件中添加。

对于动态生成的元素，可以使用如下方法对它设置唯一的ID：

```java
GrowingIO.setViewID(View view, String id);
```

{% hint style="info" %}
1. 如果在ViewGroup上设置ID的话，SDK会忽略他所有子元素的默认ID（就是写在xml文件里的）只会使用GrowingIO.setViewID设置的ID。
2. ID只能设置为字母、数字和下划线的组合。
3. 对于已经集成过旧版 SDK \(1.x\)并圈选过的应用，对某个元素设置ID后再圈选它，指标数值会从零开始计算，类似初次集成SDK后发版的效果，但不影响之前圈选的其它指标数据。如果不希望出现这种情况，请不要使用这个方法
{% endhint %}



### 忽略元素

如果您需要忽略某些特殊内容，比如倒计时元素或涉及隐私的内容，可以使用该功能。

```java
GrowingIO.ignoreView(View view)
```



### 设置元素对象

如果元素自身的内容并不能代表具体的意义，可以使用元素对象来标识。

例如：在商品`ListView`添加购物车的场景中，每个商品`item`都含有一个加入购物车按钮，这时无法区分商品`item`与将它加入购物车按钮的一对一关系，此时调用此方法增加描述。

```java
GrowingIO.setViewInfo(View view, String info);
```

{% hint style="info" %}
适用于原有`v`字段含义不大，只关注描述的场景，使用此接口后`v`字段将不采集
{% endhint %}



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

### 

### setPageVariable

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

使用此接口时，请先阅读 [**GrowingIO 对于页面的定义**](android-chang-jian-wen-ti.md#growingio-dui-yu-ye-mian-de-ding-yi)。

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
请注意这里的`activity`和`fragment`参数，[**它关乎GrowingIO对于页面的定义**](android-chang-jian-wen-ti.md#growingio-dui-yu-ye-mian-de-ding-yi)。

参数选择当前界面上最后一个初始化完成的对象引用，例如一个`Activity`中嵌套多个`Fragment`情况，当前页面最后初始化完成的是`Fragment`，请确认当前页面`Fragment`，并且得到其当前引用，`new`对象将不会发送的哦。
{% endhint %}

**参数说明：**

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| activity | Activity | 是 | **最后一个初始化完成的页面** |
| fragment | Fragment | 是 | **最后一个初始化完成的页面** |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSONObject | 否 | 页面级别的信息 |

**参数限制条件：**

参数违反以下条件将不发送数据，`SDK 2.4.0` 以上能够在 Log 日志中查看对应报错，之下版本无提示信息。调用后请关注日志，查看数据发送是否成功，事件类型`t`为`pvar`。

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
      <td style="text-align:left">pageLevelVariables</td>
      <td style="text-align:left">
        <p>非空，长度限制小于等于100（<code>pageLevelVariable.length()&lt;=100</code>）；</p>
        <p><code>pageLevelVariable</code> 内部不允许含有<code>JSONObject</code>或者<code>JSONArray</code>；</p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code>长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>**示例代码：**

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
注意确认当前页面，通过[圈选](./#yu-bei-zhi-shi)方式最快能够定位当前页面，在当前页面埋点最稳定可靠。如果页面未确认，可能在`Activity`和`Fragment`嵌套的场景下埋点失败。
{% endhint %}

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ pvar 事件验证](../growingio-debugger/best-practice.md#pvar-ye-mian-ji-bian-liang-shi-jian)
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
        <p><code>key</code> 长度限制小于等于50，<code>value</code>长度限制小等于1000，值不能为空串，也就是""。</p>
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

{% hint style="info" %}
#### 推荐您使用 MobileDebugger，我们为您列举了应用场景和验证示例，请移步查看：[ 用户变量](../growingio-debugger/best-practice.md#chang-jing-yi-yong-hu-bian-liang-zhi-deng-lu-yong-hu-id)
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

\*\*\*\*

\*\*\*\*

## 验证 SDK 是否正常工作

验证 SDK 的采集事件数据发送至关重要，数据采集是数据分析的基础，保证了数据采集的精准才能帮助您做出正确的数据分析。GrowingIO 一直在努力满足市面上所有 APP 的数据自动采集，也成为无埋点数据采集，但是因为开发者的实现方式在江湖上不统一， 我们覆盖事件采集触发逻辑可能与您的实现方式有出入，所以强烈建议开发者验证 GrowingIO SDK 的数据发送，如果有任何问题，及时反馈技术支持，我们会一直努力增强我们的采集能力，给您更好的服务。

### GrowingIO采集事件介绍

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
      <td style="text-align:left">page</td>
      <td style="text-align:left">页面浏览</td>
      <td style="text-align:left">每当进入一个页面时发送</td>
    </tr>
    <tr>
      <td style="text-align:left">imp</td>
      <td style="text-align:left">元素展现</td>
      <td style="text-align:left">页面元素发生变动的时候触发，比如弹框或者<code>ListView</code>滑动</td>
    </tr>
    <tr>
      <td style="text-align:left">clck</td>
      <td style="text-align:left">点击</td>
      <td style="text-align:left">点击实现了相关 click Listener 的元素控件，<a href="./#2-dian-ji-shi-jian-cai-ji-luo-ji">常见问题中将详细列举</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">chng</td>
      <td style="text-align:left">输入框内容变化</td>
      <td style="text-align:left">输入框<code>EditText</code>失去焦点，默认不采集输入内容</td>
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
| cstm | 自定义事件触发 | 调用[`track`](./#track)后发送 |
| pvar | 页面级变量触发 | 调用[`setPageVariable`](./#setpagevariable)后发送 |
| evar | 转化变量触发 | 调用[`setEvar`](./#setevar)后发送 |
| ppl | 设置用户变量 | 调用[`setPeopleVariable`](./#setpeoplevariable)后发送 |
| vstr | 设置访问用户变量 | 调用[`setVisitor`](./#setvisitor)后发送 |



### 验证工具

#### [1. MobileDebugger](../growingio-debugger/#qi-dong-mobile-debugger)

#### [2.查看日志](./#setdebugmode)



### 圈选功能验证

请先依据[圈选文档](../../data-definition/circle/app.md)，唤醒圈选功能，主要检查元素是否都可圈选，重点尝试WebView内部元素是否可圈。

当显示高亮则证明可圈，如图所示：

![&#x5143;&#x7D20;&#x9AD8;&#x4EAE;&#x8BC1;&#x660E;&#x53EF;&#x5708;](../../.gitbook/assets/image%20%28121%29.png)

\*\*\*\*

## 旧版本升级

{% hint style="danger" %}
#### 升级到 2.x SDK 前，请务必联系客服协助您完成后台配置的升级！
{% endhint %}

本文旨在帮助您从 1.x 版本无缝升级至 2.x 版本，由于两个版本的诸多接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

您需要完成以下几个步骤：

### 1. 更新 SDK 版本号为最新版本

请您参考以下开发文档，完成SDK初始化代码的添加。

* [Android SDK 集成](./#ji-cheng)

Tips：建议您在开发中，使用 `DebugMode` 或者 [**Mobile Debugger**](../growingio-debugger/#shi-yong-mobile-debugger-ce-shi-shu-ju) 校验 GrowingIO SDK 的数据是否正常上传，其中开启`DebugMode`的方式见[调试查看发送数据日志](./#7-tiao-shi-cha-kan-fa-song-shu-ju-ri-zhi)。

### **2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至[“用户变量”](./#setuserid)。两者的区别主要在于：用户变量支持自定义的归因方式。

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

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../../data-definition/page-variable.md#zi-ding-yi-bian-liang-de-pei-zhi-he-shang-chuan)。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量](../../data-model/event-model/autotrack-event/page-events-and-properties.md#ye-mian-shi-jian-de-ding-yi)”。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

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

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。

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

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。[点击查看 GrowingIO Mobile Debugger 的安装和使用](../growingio-debugger/#growingio-mobile-debugger)。

####  {#下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要}

