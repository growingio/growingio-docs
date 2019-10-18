# Android SDK 常见问题

## 无埋点数据采集问题

### 1. GrowingIO 对于页面的定义

**确认当前页面方法有三种：**

1.[圈选](../../data-definition/circle/app.md)，查看圈选页面为当前页面![](https://docs.growingio.com/.gitbook/assets/-LGNxeGABUADKiTWTaEM-LI58sGTg1USJzrnTVZD-LI5KdIC78J2Y7tfdBp2image.png)

2.[查看日志](android-sdk.md#setdebugmode)，进入页面发送的`page`的`p`为当前的页面

3.使用[`Mobile Debugger`](../growingio-debugger/#growingio-mobile-debugger)查看`page`事件的`p`



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

如果您自定义了 Click 事件， 但是希望 SDK 采集。 可以放置一个 `onClickListener` 作为代理。这种方案即使随着我们的 SDK 升级也会被兼容。

{% hint style="warning" %}
请注意设置您的 View 可点击： 

```java
//设置view Clickable, 如果不设置会不能圈选这个 View。
view.setClickable(true);
```
{% endhint %}

```java
public void onCustomClick(View view){
	// 您的业务
	...	
	// 为了 GrowingIO 能够采集自定义点击事件，调用 android.view.OnClickListener
    new View.OnClickListener() {
        @Override
        public void onClick(View view) {}
    }.onClick(view);
}
```

例： `TabHost` 的点击事件采集增加 onClickListener 后可以采集到点击事件

```java
TabHost.OnTabChangeListener listener = new TabHost.OnTabChangeListener() {
    public void onTabChanged(String tabId) {
        // GrowingIO 点击事件采集适配 TabHost 添加代码
        new View.OnClickListener() {
            @Override
            public void onClick(View view) {}
        }.onClick(mTabHost.getCurrentTabView());
    }
}
```

最后，如果您是在布局文件中在`view`上使用 `onClick` 属性的点击事件，不会被采集，不支持。

{% hint style="danger" %}
如果您还未采集到点击事件， 并且使用了 lambda 表达式，请看 [lambda 配置](android-sdk.md#5-lambda-biao-da-shi-zhi-chi-pei-zhi-xiang)。
{% endhint %}



### 3. 如何查看当前 APP SDK 版本

有以下多种方式，任选其一。

1. 唤起圈选，点击小红点，能够看到版本号；
2. 使用 Mobile Debugger ， 点击左侧截图区域的 `i` 图标，能够看到版本号；
3. 翻阅代码，app 目录层中的 build.gradle 文件中查找；
4. 查看日志，每条 vst 事件中 `av` 字段描述版本号；
5. 抓包查看，网络请求中包含。



### 4. 为何不建议自定义设备ID

我们强烈不建议您自行定义设备ID有以下几个方面：

1. 我们采集的设备 ID 为了能够唯一标识一台设备信息，如果您进行自定义，有可能用户卸载重新安装应用，设备 ID 会不一致，造成老用户被识别成新用户；
2. 如果您未曾定义过设备ID，并且已经集成SDK并且发版过，则新旧设备 ID 不兼容，老用户被认为成新用户，导致新用户数量暴增；



### 5. 多个 URLScheme 配置

在 Android 无埋点 SDK 集成步骤中，其中有一步需要在 manifest.xml 文件中配置 `intent-filter` 代码块，如果有多个 `URLScheme` 配置，请参照以下代码：

```markup
<activity
    android:name=".LauncherActivity"
    android:launchMode="singleTop"
    android:theme="@style/AppTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <!-- GrowingIO URLScheme -->
    <intent-filter>
        <data android:scheme="growing.xxxxxxxxxxxxxxxx" />
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>
    <!-- 测试多个 scheme 能够成功被唤醒 -->
    <intent-filter>
        <data
            android:host="share"
            android:scheme="will" />
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>
</activity>
```



## 埋点 API 使用问题

### 自定义页面变量事件发送失败

此问题常发生在 `Activity` 中包含多个 `Fragment` 的页面中。

* 首先需要确认当前页面发送的页面访问\(`page`\)事件是哪个 `Fragment` ，您可以[参照这三种方式确认当前的页面访问事件](android-chang-jian-wen-ti.md#1-growingio-dui-yu-ye-mian-de-ding-yi)。
* 如果此 `Fragment` 不是您业务上认为的页面，可以使用 `ignoreFragment` 不认为此 Fragment 可以作为页面访问事件发送。
* 注意：`setPageVariable` 接口参数中的 Activity 或者 Fragment **必须为当前页面访问事件页面**，即可成功发送自定义页面事件。



## SDK 性能问题

### 1. SDK 编译时性能和消耗时间

{% hint style="success" %}
**SDK 2.7.5** **正式支持 instant Run， 不需要配置 gioenable 变量，请升级 SDK。**
{% endhint %}

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

{% hint style="danger" %}
SDK 2.7.4 以下版本不支持 Instant Run , 请开发者开发期间配置 `gioenable = false` ，即可使用 Instant Run。
{% endhint %}



### 2. SDK 数据发送策略

如下图：

![](../../.gitbook/assets/image%20%286%29.png)

## 圈选问题

### 1. 无法圈选

请根据[这篇文档](../../faq/faq-circle.md#3-sao-miao-quan-xuan-er-wei-ma-dan-shi-wu-fa-zheng-chang-quan-xuan)自行排查，如果仍有问题，可以联系技术支持。

### 2. 集成 SDK 后，Activity 受影响

是否在集成 SDK 时配置 GrowingIO `<intent-filter/>` 代码块中的 Activity 中使用了 `getIntent` 呢？

解答： 请在您的逻辑中过滤掉 Gio 的 Intent ，我们的 Intent 大致格式是这样：

`growing.xxxxxxxxxxx://growing/oauth2/token?loginToken=xxxxx.........`

请您在代码逻辑中判断如果是以 `growing` 开头的 `scheme` 不处理它，代码示例：

```java
// 请注意自己判断 intent 是否为空
Uri data = intent.getData();
if (data.getScheme().startsWith("growing.")){    
    Log.d(TAG, "GrowingIO url scheme, not process");
}
```



## Gradle 常见问题

### 1. Gradle 支持版本

Android 无埋点 SDK 支持 `com.android.tools.build:gradle` **2.3.3** 及其以上版本，推荐版本是 **3.2.1**。下文将简称 `com.android.tools.build:gradle`为 gradle。

无埋点 SDK 随着 gradle 的升级也在逐步兼容，请参考以下表格更新 SDK：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x65E0;&#x57CB;&#x70B9; SDK &#x7248;&#x672C;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">2.8.4+</td>
      <td style="text-align:left">
        <ul>
          <li>SDK 2.8.4 &#x7248;&#x672C;&#x53CA;&#x5176;&#x4EE5;&#x4E0A;&#x652F;&#x6301;
            gradle 3.5.0&#xFF1B;</li>
          <li>&#x65B0;&#x589E;&#x5728;&#x7F16;&#x8BD1;&#x671F;&#x95F4;&#x6821;&#x9A8C;&#x7528;&#x6237;&#x4F7F;&#x7528;&#x7684;
            SDK &#x662F;&#x5426;&#x4E3A;&#x7A33;&#x5B9A; SDK &#x7248;&#x672C;&#xFF0C;&#x5982;&#x679C;&#x4F7F;&#x7528;&#x6709;&#x95EE;&#x9898;&#x7684;&#x7248;&#x672C;&#x5C06;&#x4E3B;&#x52A8;&#x5728;&#x7F16;&#x8BD1;&#x65F6;&#x63D0;&#x793A;&#x7528;&#x6237;&#x5347;&#x7EA7;
            SDK&#xFF0C;&#x5E76;&#x629B;&#x51FA;&#x5F02;&#x5E38;&#x63D0;&#x793A;&#xFF0C;&#x62D2;&#x7EDD;&#x7F16;&#x8BD1;&#x3002;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">2.7.8</td>
      <td style="text-align:left">
        <ul>
          <li>&#x589E;&#x52A0;&#x63A5;&#x53E3; <code>setAndroidIdEnable</code> , <code>setImeiEnable</code>, <code>setGoogleAdIdEnable</code> &#x4E3A;&#x6D77;&#x5916;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#x6D89;&#x53CA;&#x91C7;&#x96C6;&#x7528;&#x6237; <code>androidId</code>, <code>imei</code>, <code>googleAdId</code> &#x9690;&#x79C1;&#x6570;&#x636E;&#x7684;&#x5F00;&#x5173;&#x652F;&#x6301;&#x3002;
            <a
            href="api.md#gradle-bian-yi-shi-pei-zhi-api">&#x8BE6;&#x60C5;&#x89C1; API &#x6587;&#x6863;</a>&#x3002;<b>&#x6CE8;&#x610F;&#xFF0C;&#x4EE5;&#x4E0A;&#x63A5;&#x53E3;&#x4E0D;&#x652F;&#x6301; gradle 3.0.x &#x4EE5;&#x4E0B;&#x7248;&#x672C;</b>&#x3002;</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

### 2. 升级 2.8.4 编译报错

升级无埋点 SDK 2.8.4 如果报错如下：

> Unable to load class **'org.apache.http.impl.client.CloseableHttpClient'**. Possible causes for this unexpected error include: Gradle's dependency cache may be corrupt \(this sometimes occurs after a network connection timeout.\) Re-download dependencies and sync project \(requires network\)
>
> The state of a Gradle build process \(daemon\) may be corrupt. Stopping all Gradle daemons may solve this problem. Stop Gradle build processes \(requires restart\)
>
> Your project may be using a third-party plugin which is not compatible with the other plugins in the project or the version of Gradle requested by the project.
>
> In the case of corrupt Gradle processes, you can also try closing the IDE and then killing all Java processes.

此报错原因是在 SDK 2.8.4 时新增在编译期间校验用户使用的 SDK 是否为稳定 SDK 版本，如果使用有问题的版本将主动在编译时提示用户升级 SDK，并抛出异常提示，拒绝编译功能，此部分代码逻辑触发了Proguard 的 Bug ，导致报错。建议解决方案有两种：

1. 升级 gradle 为 3.2.1 及其以上版本；
2. 在项目级别 build.gradle 中添加以下依赖即可：

```text
 classpath "org.apache.httpcomponents:httpclient:4.5.10"
```

示例代码：

```groovy
dependencies {
    //使用 gradle 3.2.1 以下并且 SDK 2.8.4 版本会触发此问题
    classpath 'com.android.tools.build:gradle:3.1.4'
    classpath 'com.growingio.android:vds-gradle-plugin:autotrack-2.8.4'
    //添加以下依赖能够解决，或者升级 gradle
    classpath "org.apache.httpcomponents:httpclient:4.5.10"
}
```

 

### 3. 使用 Jack 注意

依据[官网说明](https://developer.android.com/studio/write/java8-support#migrate)，SDK不支持 Jack 编译器，请确认移除 jackOptions 代码块。

```groovy
android {
    ...
    defaultConfig {
        ...
        // Remove this block. 删掉这里！！！
        jackOptions {
            enabled true
            ...
        }
    }
    // Keep the following configuration in order to target Java 8.
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

如果您未移除，集成 SDK 后 App 将 Crash 。



## APP 内嵌 H5 页面常见问题

### H5 页面需要集成 SDK 么？

如果您集成原生无埋点 SDK ， 则不需要单独集成 hybrid sdk ，无埋点 sdk 会将 hybrid sdk 自动注入，并且采集点击、浏览、文本输入等事件。

### H5 页面什么情况需要集成 web js sdk ？

如果您的页面，在移动端和网站同时投放，不考虑平台只关注这个页面的数据时，集成 web js sdk 。

此时，由于您集成了 web js sdk ，并且移动端无埋点 sdk 会自动注入 hybrid sdk ，则会发两份数据采集信息，web js sdk 发送一份， hybrid sdk  发送一份。 

如果您单独期望关闭 web js sdk 的数据采集，请将`window.webViewRequestSend`的值为`false`。

### H5 怎么增加自定义事件和变量 ？

[Hybrid SDK 埋点文档。](../hybrid-yong-hu-ji-cheng-wen-dang.md#da-dian-shi-jian-yuan-sheng-sdk-22-kai-shi-zhi-chi)

[Web JS SDK 埋点文档。](../web-js-sdk/web-js-sdk-api.md)





