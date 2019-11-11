---
description: >-
  GrowingIO 提供了初始化配置项 API 和运行时 API 来自定义 SDK
  的采集，满足不同场景的定制采集。同一种含义的API，只需要调用一次即可，比如您关闭SDK的采集功能，可以在初始化中配置，也可以在运行时配置，只需调用一次相关API即可。
---

# Android 无埋点 SDK API

## Gradle 编译时配置 API

使您的应用在编译时即可自定义。

{% hint style="info" %}
Android 2.7.8 SDK 为海外上架应用涉及采集用户 `androidId`, `imei`, `googleAdId` 隐私数据的开关支持。

增加分别可以在编译时、SDK 初始化、 APP 运行时调用的对应接口。
{% endhint %}

{% hint style="danger" %}
**注意，imeiEnable、androidEnable、googleAdIdEnable 配置项不支持  com.android.tools.build:gradle  3.0.x 以下版本**。
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x7F16;&#x8BD1;&#x65F6;&#x914D;&#x7F6E;&#x9879;API</th>
      <th style="text-align:left">&#x9ED8;&#x8BA4;&#x503C;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x7248;&#x672C;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">imeiEnable</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>&#x8BBE;&#x7F6E;&#x4E3A; false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; <code>IMEI</code> &#x3002;&#x5728;&#x7F16;&#x8BD1;&#x671F;&#x914D;&#x7F6E;&#x5C06;&#x5220;&#x9664;&#x8BE5;&#x90E8;&#x5206;&#x7684;&#x91C7;&#x96C6;&#x4EE3;&#x7801;&#xFF0C;&#x540E;&#x7EED;&#x914D;&#x7F6E;&#xFF08;&#x521D;&#x59CB;&#x5316;&#x6216;&#x8005;&#x8FD0;&#x884C;&#x65F6;&#xFF09;&#x5C06;&#x5931;&#x6548;&#x3002;</b>
      </td>
      <td style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">androidIdEnable</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>&#x8BBE;&#x7F6E;&#x4E3A; false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; <code>androidId</code> &#x3002;&#x5728;&#x7F16;&#x8BD1;&#x671F;&#x914D;&#x7F6E;&#x5C06;&#x5220;&#x9664;&#x8BE5;&#x90E8;&#x5206;&#x7684;&#x91C7;&#x96C6;&#x4EE3;&#x7801;&#xFF0C;&#x540E;&#x7EED;&#x914D;&#x7F6E;&#xFF08;&#x521D;&#x59CB;&#x5316;&#x6216;&#x8005;&#x8FD0;&#x884C;&#x65F6;&#xFF09;&#x5C06;&#x5931;&#x6548;&#x3002;</b>
      </td>
      <td style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">googleAdIdEnable</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>&#x8BBE;&#x7F6E;&#x4E3A; false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; <code>GoogleAdId</code> &#xFF0C;&#x5728;&#x7F16;&#x8BD1;&#x671F;&#x914D;&#x7F6E;&#x5C06;&#x5220;&#x9664;&#x8BE5;&#x90E8;&#x5206;&#x7684;&#x91C7;&#x96C6;&#x4EE3;&#x7801;&#xFF0C;&#x540E;&#x7EED;&#x914D;&#x7F6E;&#xFF08;&#x521D;&#x59CB;&#x5316;&#x6216;&#x8005;&#x8FD0;&#x884C;&#x65F6;&#xFF09;&#x5C06;&#x5931;&#x6548;&#x3002;</b>
      </td>
      <td style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">oaidEnable</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">
        <p>&#x56FD;&#x5185;<a href="http://www.msa-alliance.cn/col.jsp?id=120">&#x79FB;&#x52A8;&#x5B89;&#x5168;&#x8054;&#x76DF;MSA</a> &#x8054;&#x5408;&#x5404;&#x5927;&#x624B;&#x673A;&#x5236;&#x9020;&#x5546;&#x63A8;&#x51FA;&#x4E86;
          OAID &#xFF0C; &#x4F5C;&#x4E3A;&#x552F;&#x4E00;&#x5E7F;&#x544A;&#x6807;&#x8BC6;&#x7B26;&#x3002;</p>
        <p><b>&#x8BBE;&#x7F6E;&#x4E3A; false &#x5219;&#x8FD0;&#x884C;&#x65F6;&#x548C;&#x521D;&#x59CB;&#x5316;&#x7684;&#x914D;&#x7F6E;&#x5219;&#x65E0;&#x6548;&#x3002;</b>
        </p>
      </td>
      <td style="text-align:left">2.8.5&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
  </tbody>
</table>**示例代码**

```groovy
android { · · · }
// 须位于 android 代码块下
growingio {
    defaultConfig { 
        imeiEnable true 
        androidIdEnable true 
        googleAdIdEnable true 
    }
    buildTypes {
        googlePlay { 
            imeiEnable false
            ndoridIdEnable false 
            googleAdIdEnable true 
        }
    }
}
```

## 初始化配置项 API

初始化配置项均在`Application`的`onCreate`方法中 SDK 初始化代码块中设置，下面将分类并描述含义。

#### 示例代码

```java
public class MyApplication extends Application {    @Override    public void onCreate() {        GrowingIO.startWithConfiguration(this,new Configuration()            .disableCellularImp()            .disableImageViewCollection(false)            .setBulkSize(100)            .setCellularDataLimit(1000)            .setChannel("渠道号")            .setDebugMode(true)            // 2.8.4 新增 appAwakePassedTime 参数: 单位毫秒，App 唤醒到收到 GIO callback 的时间，用以判断网络状态不好的情况，应用已经打开很久，才收到回调，开发人员决定是否收到参数后仍然跳转自定义的指定页面。当返回值为 0 的时候，为 DeepLink 方式打开。            .setDeeplinkCallback(new DeeplinkCallback() {                @Override                public void onReceive(Map<String, String> params, int error, long appAwakePassedTime) {                        }            })            .setDiagnose(false)            .setDisabled(false)            .setDisableImpression(false)            .setFlushInterval(1000)            .setMutiprocess(true)            .setSampling(0.34)            .setSessionInterval(23000)            .setTestMode(true)            .setThrottle(false)            .setTrackWebView(true)            .supportMultiProcessCircle(true)            .trackAllFragments()            //以下 Android 2.7.8 新增            .setImeiEnable(true)            .setGoogleAdIdEnable(true)            .setAndroidIdEnable(true)            //以下 Android 2.8.5 新增            .setOAIDEnable(true)        );    }}
```



### 基础配置 API

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x521D;&#x59CB;&#x5316;&#x914D;&#x7F6E;&#x9879;API</th>
      <th style="text-align:left">&#x9ED8;&#x8BA4;&#x503C;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x6700;&#x4F4E;&#x7248;&#x672C;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">setDebugMode</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">&#x5728;Logcat&#x4E2D;&#x8F93;&#x51FA;&#x91C7;&#x96C6;&#x65E5;&#x5FD7;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setTestMode</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">
        <p>&#x5B9E;&#x65F6;&#x53D1;&#x9001;&#x6570;&#x636E;&#xFF0C;&#x5F00;&#x542F;&#x5219;&#x4E0D;&#x9075;&#x5FAA;&#x79FB;&#x52A8;&#x7F51;&#x7EDC;&#x72B6;&#x6001;&#x4E0B;&#x6570;&#x636E;&#x53D1;&#x9001;&#x5927;&#x5C0F;&#x9ED8;&#x8BA4;
          3M &#x9650;&#x5236;&#x4EE5;&#x53CA;&#x91C7;&#x96C6;&#x6570;&#x636E;&#x7F13;&#x5B58;30&#x79D2;&#x53D1;&#x9001;&#x7B56;&#x7565;&#x3002;</p>
        <p>&#x4E3A;&#x4E86;&#x65B9;&#x4FBF;&#x5F00;&#x53D1;&#x8005;&#x67E5;&#x770B;&#x65E5;&#x5FD7;&#xFF0C;&#x4E00;&#x822C;&#x548C;<code>setDebugMode</code>&#x4E00;&#x8D77;&#x4F7F;&#x7528;&#x3002;</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setChannel</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6E20;&#x9053;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">useID</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5728;&#x8BA1;&#x7B97;<code>xpath</code>&#x65F6;&#x4F7F;&#x7528;&#x63A7;&#x4EF6;<code>id&#xFF0C;</code>&#x9ED8;&#x8BA4;&#x4F7F;&#x7528;</td>
      <td
      style="text-align:left">2.6.0&#x4E2D;&#x5220;&#x9664;</td>
    </tr>
  </tbody>
</table>

### SDK 功能API

| 初始化配置项API | 默认值                    | 说明 | 版本                           |
| :--- | :--- | :--- | :--- |
| setDeeplinkCallback | 无 | DeepLink 回调接口，获得自定义参数以便跳转对应 APP页 面 | 2.3.2 以上 |
| setTrackWebView | true | 是否采集全部的`WebView,`设置为`false`时不采所有`WebView`数据 |  |
| supportMultiProcessCircle | false | 是否使用多进程圈选功能 |  |
| setMutiprocess | false | 使用了多进程必须配置，自定义事件和变量值才会多进程共享 |  |
| trackAllFragments | false | 是否采集所有Fragment |  |
| setHashTagEnable | false | 在`WebView`中的页面访问，是否认为点击锚点链接是一个页面浏览 |  |



### 数据采集发送 API

| 初始化配置项API | 默认值                                                        | 说明 |
| :--- | :--- | :--- |
| setDisabled | false | SDK 是否采集数据，设置为`true`时不采集数据 |
| setSampling | 1 | 采样率\[0.01~1\],若设置sampling = 0.01，则 1% 的设备会被采集数据，每次启动会根据用户设置的采样率判断设备是否在采集的范围之内，**使用之前请咨询技术支持** |
| setSessionInterval | 30 \* 1000 | 在后台停留时长超过此值，则产生新的`sessionId`,发送`visit`事件。 |
| setFlushInterval | 30 \* 1000 | 数据刷新的最长时间间隔，默认30 秒 。如果距离上次发送数据事件超过此时间则发送事件 |
| setThrottle | false | 是否节流发送，节流发送时`imp`不发送，不发送但是采集，`imp`为元素展示事件 |
| setDisableImpression | false | 是否采集`imp`事件，`imp`为元素展示事件，默认采集`imp` |
| disableCellularImp | false | 否关闭移动蜂窝网`imp`事件采集，`imp`为元素展示事件 |
| setCellularDataLimit | 3 \* 1024 \* 1024 | 一天的时间之内，在移动蜂窝网下的数据最大传输量，默认3M。 |
| setBulkSize | 300 | 如果数据库存储数据条数大于等于`bulkSize`，则马上发送数据。 |
| setImeiEnable | true | 为了海外应用市场上架应用，**Android 2.7.8** 新增初始化配置接口，设置为 false 则 SDK 不采集 `imei`  |
| setAndroidIdEnable | true | 为了海外应用市场上架应用，**Android 2.7.8** 新增初始化配置接口，设置为 false 则 SDK 不采集 `androidid` 。 |
| setGoogleAdIdEnable | true | 为了海外应用市场上架应用，**Android 2.7.8** 新增初始化配置接口，设置为 false 则 SDK 不采集 `GoogleAdId` 。 |
| setOAIDEnable | true | 国内[移动安全联盟MSA](http://www.msa-alliance.cn/col.jsp?id=120) 联合各大手机制造商推出了 OAID ， 作为唯一广告标识符。**Android 2.8.5** 新增。 |





## GrowingIO 运行时 API

GrowingIO 为 APP 提供运行时随意调用的 API，使用方法如下：

```java
// 得到 GrowingIO 实例后可以调用其中 APIGrowingIO gio = GrowingIO.getInstance();gio.setUserId("张溪梦");
```

{% hint style="danger" %}
GrowingIO 所有 API 都需要在主线程调用。
{% endhint %}



### 基础配置 API

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x8FD0;&#x884C;&#x65F6;API</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">setGeoLocation</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x7ECF;&#x7EAC;&#x5EA6;&#xFF0C;&#x5E76;&#x5728; vst &#x4E8B;&#x4EF6;&#x4E2D;&#x53D1;&#x9001;&#xFF0C;Android
        SDK &#x6682;&#x65F6;&#x6CA1;&#x529E;&#x6CD5;&#x81EA;&#x52A8;&#x83B7;&#x53D6;<code>GPS</code>&#x6570;&#x636E;&#xFF0C;&#x5982;&#x679C;&#x60A8;&#x8981;&#x91C7;&#x96C6;<code>GPS</code>&#x6570;&#x636E;&#xFF0C;&#x9700;&#x8981;&#x5728;&#x60A8;&#x7684;App&#x6BCF;&#x6B21;&#x83B7;&#x53D6;&#x5B8C;<code>GPS</code>&#x6570;&#x636E;&#x4E4B;&#x540E;&#xFF0C;&#x901A;&#x8FC7;&#x8BE5;<code>API</code>&#x544A;&#x77E5;
        SDK&#x3002;&#x5982;&#x679C;&#x60A8;&#x4E0D;&#x8BBE;&#x7F6E;&#xFF0C;&#x6211;&#x4EEC;&#x9ED8;&#x8BA4;&#x4F7F;&#x7528;&#x7528;&#x6237;<code>IP</code>&#x7684;<code>Location&#x3002;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">clearGeoLocation</td>
      <td style="text-align:left">&#x6E05;&#x7A7A;&#x7ECF;&#x7EAC;&#x5EA6;</td>
    </tr>
    <tr>
      <td style="text-align:left">setViewInfo</td>
      <td style="text-align:left">
        <p>&#x914D;&#x7F6E; view &#x7684; Tag&#xFF0C;&#x6807;&#x8BB0; View &#xFF0C;&#x5E76;&#x5728;
          GrowingIO &#x76F8;&#x5173;&#x4E8B;&#x4EF6;&#x4E2D;&#x53D1;&#x9001; &#xFF0C;&#x5185;&#x5BB9;&#x5BF9;&#x5E94; <code>xPath</code> &#x4E2D;&#x7684; <code>obj</code>
        </p>
        <p>&#x4F8B;&#x5982;&#xFF1A;&#x5728;&#x5546;&#x54C1;<code>ListView</code>&#x6DFB;&#x52A0;&#x8D2D;&#x7269;&#x8F66;&#x7684;&#x573A;&#x666F;&#x4E2D;&#xFF0C;&#x6BCF;&#x4E2A;&#x5546;&#x54C1;<code>item</code>&#x90FD;&#x542B;&#x6709;&#x4E00;&#x4E2A;&#x52A0;&#x5165;&#x8D2D;&#x7269;&#x8F66;&#x6309;&#x94AE;&#xFF0C;&#x8FD9;&#x65F6;&#x65E0;&#x6CD5;&#x533A;&#x5206;&#x5546;&#x54C1;<code>item</code>&#x4E0E;&#x5C06;&#x5B83;&#x52A0;&#x5165;&#x8D2D;&#x7269;&#x8F66;&#x6309;&#x94AE;&#x7684;&#x4E00;&#x5BF9;&#x4E00;&#x5173;&#x7CFB;&#xFF0C;&#x6B64;&#x65F6;&#x8C03;&#x7528;&#x6B64;&#x65B9;&#x6CD5;&#x589E;&#x52A0;&#x63CF;&#x8FF0;&#x3002;</p>
        <p>&#x6CE8;&#x610F;&#xFF1A;&#x9002;&#x7528;&#x4E8E;&#x539F;&#x6709;<code>v</code>&#x5B57;&#x6BB5;&#x542B;&#x4E49;&#x4E0D;&#x5927;&#xFF0C;&#x53EA;&#x5173;&#x6CE8;&#x63CF;&#x8FF0;&#x7684;&#x573A;&#x666F;&#xFF0C;&#x4F7F;&#x7528;&#x6B64;&#x63A5;&#x53E3;&#x540E;<code>v</code>&#x5B57;&#x6BB5;&#x5C06;&#x4E0D;&#x91C7;&#x96C6;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setViewContent</td>
      <td style="text-align:left">
        <p>&#x914D;&#x7F6E; view &#x7684; Tag&#xFF0C;&#x6807;&#x8BB0; View &#xFF0C;&#x5E76;&#x5728;
          GrowingIO&#x76F8;&#x5173;&#x4E8B;&#x4EF6;&#x4E2D;&#x53D1;&#x9001;&#xFF0C;&#x5185;&#x5BB9;&#x5BF9;&#x5E94; <code>xPath</code> &#x4E2D;&#x7684; <code>v</code>
        </p>
        <p>SDK&#x9ED8;&#x8BA4;&#x4E0D;&#x4F1A;&#x91C7;&#x96C6;ImageView&#x7684;&#x5185;&#x5BB9;&#xFF0C;&#x4E3A;&#x4E86;&#x80FD;&#x5BF9;&#x4E0D;&#x540C;&#x7684;&#x56FE;&#x7247;&#x5143;&#x7D20;&#xFF08;ImageView&#xFF09;&#x533A;&#x5206;&#x7EDF;&#x8BA1;&#xFF0C;&#x9700;&#x8981;&#x5BF9;&#x6BCF;&#x4E2A;&#x5177;&#x6709;&#x5206;&#x6790;&#x610F;&#x4E49;&#x7684;&#x56FE;&#x7247;&#x5143;&#x7D20;&#xFF08;ImageView&#xFF09;&#x6DFB;&#x52A0;&#x63CF;&#x8FF0;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setViewID</td>
      <td style="text-align:left">
        <p>&#x8BBE;&#x7F6E; View id &#xFF0C;&#x914D;&#x7F6E;&#x4E4B;&#x540E;&#x5BF9;&#x5E94;
          xPath &#x4E2D;&#x7684; view id&#xFF0C;SDK&#x5C06;&#x4F1A;&#x4F7F;&#x7528;Layout&#x6587;&#x4EF6;&#x4E2D;&#x7684;ID&#x6765;&#x8BC6;&#x522B;&#x4E00;&#x4E2A;&#x5143;&#x7D20;&#x3002;</p>
        <p>&#x5982;&#x679C;&#x90E8;&#x5206;&#x5143;&#x7D20;&#x5728;Layout&#x6587;&#x4EF6;&#x4E2D;&#x6CA1;&#x6709;ID&#xFF0C;&#x5EFA;&#x8BAE;&#x5728;Layout&#x6587;&#x4EF6;&#x4E2D;&#x6DFB;&#x52A0;&#x3002;</p>
        <p>&#x5BF9;&#x4E8E;&#x52A8;&#x6001;&#x751F;&#x6210;&#x7684;&#x5143;&#x7D20;&#xFF0C;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x5982;&#x4E0B;&#x65B9;&#x6CD5;&#x5BF9;&#x5B83;&#x8BBE;&#x7F6E;&#x552F;&#x4E00;&#x7684;ID&#x3002;</p>
        <p>&#x5F53;&#x60A8;&#x7684;&#x5E94;&#x7528;&#x754C;&#x9762;&#x6539;&#x7248;&#x65F6;&#xFF0C;&#x53EF;&#x80FD;&#x4F1A;&#x5BFC;&#x81F4;&#x65E0;&#x6CD5;&#x51C6;&#x786E;&#x5730;&#x7EDF;&#x8BA1;&#x5DF2;&#x7ECF;&#x5708;&#x9009;&#x7684;&#x5143;&#x7D20;&#x3002;&#x56E0;&#x6B64;&#xFF0C;&#x5BF9;&#x4E8E;&#x5E94;&#x7528;&#x4E2D;&#x7684;&#x4E3B;&#x8981;&#x6D41;&#x7A0B;&#x6D89;&#x53CA;&#x5230;&#x7684;&#x754C;&#x9762;&#x5143;&#x7D20;&#xFF0C;&#x5EFA;&#x8BAE;&#x60A8;&#x4E3A;&#x5B83;&#x4EEC;&#x8BBE;&#x7F6E;&#x56FA;&#x5B9A;&#x7684;&#x552F;&#x4E00;ID&#xFF0C;&#x4EE5;&#x4FDD;&#x8BC1;&#x6570;&#x636E;&#x7684;&#x4E00;&#x81F4;&#x6027;&#x3002;</p>
        <ul>
          <li>ID &#x53EA;&#x80FD;&#x8BBE;&#x7F6E;&#x4E3A;&#x5B57;&#x6BCD;&#x3001;&#x6570;&#x5B57;&#x548C;&#x4E0B;&#x5212;&#x7EBF;&#x7684;&#x7EC4;&#x5408;</li>
          <li>&#x5982;&#x679C;&#x5728;ViewGroup&#x4E0A;&#x8BBE;&#x7F6E;ID&#x7684;&#x8BDD;&#xFF0C;SDK&#x4F1A;&#x5FFD;&#x7565;&#x4ED6;&#x6240;&#x6709;&#x5B50;&#x5143;&#x7D20;&#x7684;&#x9ED8;&#x8BA4;ID&#xFF08;&#x5C31;&#x662F;&#x5199;&#x5728;xml&#x6587;&#x4EF6;&#x91CC;&#x7684;&#xFF09;&#x53EA;&#x4F1A;&#x4F7F;&#x7528;GrowingIO.setViewID&#x8BBE;&#x7F6E;&#x7684;ID&#x3002;</li>
          <li>&#x5BF9;&#x4E8E;&#x5DF2;&#x7ECF;&#x96C6;&#x6210;&#x8FC7;&#x65E7;&#x7248;SDK&#x5E76;&#x5708;&#x9009;&#x8FC7;&#x7684;&#x5E94;&#x7528;&#xFF0C;&#x5BF9;&#x67D0;&#x4E2A;&#x5143;&#x7D20;&#x8BBE;&#x7F6E;ID&#x540E;&#x518D;&#x5708;&#x9009;&#x5B83;&#xFF0C;&#x6307;&#x6807;&#x6570;&#x503C;&#x4F1A;&#x4ECE;&#x96F6;&#x5F00;&#x59CB;&#x8BA1;&#x7B97;&#xFF0C;&#x7C7B;&#x4F3C;&#x521D;&#x6B21;&#x96C6;&#x6210;SDK&#x540E;&#x53D1;&#x7248;&#x7684;&#x6548;&#x679C;&#xFF0C;&#x4F46;&#x4E0D;&#x5F71;&#x54CD;&#x4E4B;&#x524D;&#x5708;&#x9009;&#x7684;&#x5176;&#x5B83;&#x6307;&#x6807;&#x6570;&#x636E;&#x3002;&#x5982;&#x679C;&#x4E0D;&#x5E0C;&#x671B;&#x51FA;&#x73B0;&#x8FD9;&#x79CD;&#x60C5;&#x51B5;&#xFF0C;&#x8BF7;&#x4E0D;&#x8981;&#x4F7F;&#x7528;&#x8FD9;&#x4E2A;&#x65B9;&#x6CD5;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setChannel</td>
      <td style="text-align:left"><b>2.6.5 &#x4E4B;&#x524D;&#x7248;&#x672C;</b>&#xFF1A;
        <br />&#x5148;&#x8BBE;&#x7F6E;&#x6E20;&#x9053;&#x4FE1;&#x606F;&#xFF0C;&#x518D;&#x53D1;&#x9001;&#x6570;&#x636E;
        <br
        />&#x80FD;&#x591F;&#x4FDD;&#x8BC1;&#x6240;&#x6709;&#x6570;&#x636E;&#x90FD;&#x4E00;&#x5B9A;&#x4F1A;&#x5E26;&#x4E0A;&#x6E20;&#x9053;&#x4FE1;&#x606F;
        <br
        />
        <br /><b>2.6.5 &#x53CA;&#x4E4B;&#x540E;&#x7248;&#x672C;</b>&#xFF1A;
        <br />&#x4FDD;&#x7559;&#x539F;&#x6709;&#x8BBE;&#x7F6E;&#x6E20;&#x9053;&#x4FE1;&#x606F;&#x7684;&#x65B9;&#x6CD5;&#xFF0C;&#x65B0;&#x589E;&#x5728;&#x8FD0;&#x884C;&#x65F6;&#x8BBE;&#x7F6E;&#x6E20;&#x9053;&#x4FE1;&#x606F;
        <br
        />&#x65B0;&#x589E;&#x7684;&#x63A5;&#x53E3;&#x65E0;&#x6CD5;&#x4FDD;&#x8BC1;&#x6240;&#x6709;&#x6570;&#x636E;&#x90FD;&#x4E00;&#x5B9A;&#x4F1A;&#x5E26;&#x4E0A;&#x6E20;&#x9053;&#x4FE1;&#x606F;&#xFF08;&#x867D;&#x7136;&#x6211;&#x4EEC;&#x4F1A;&#x901A;&#x8FC7;&#x91CD;&#x53D1;&#x673A;&#x5236;&#x8FDB;&#x884C;&#x4FDD;&#x8BC1;&#xFF0C;&#x4F46;&#x662F;&#x65E0;&#x6CD5;&#x505A;&#x5230;100%&#xFF09;</td>
    </tr>
  </tbody>
</table>

### 数据采集 API 

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x8FD0;&#x884C;&#x65F6;API</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x7248;&#x672C;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">disableDataCollect</td>
      <td style="text-align:left">&#x9075;&#x5B88;&#x6B27;&#x6D32;&#x8054;&#x76DF;&#x51FA;&#x53F0;&#x7684;&#x901A;&#x7528;&#x6570;&#x636E;&#x4FDD;&#x62A4;&#x6761;&#x4F8B;&#xFF0C;&#x7528;&#x6237;&#x4E0D;&#x6388;&#x6743;&#xFF0C;&#x4E0D;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x6570;&#x636E;</td>
      <td
      style="text-align:left">2.3.2 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">enableDataCollect</td>
      <td style="text-align:left">&#x9075;&#x5B88;&#x6B27;&#x6D32;&#x8054;&#x76DF;&#x51FA;&#x53F0;&#x7684;&#x901A;&#x7528;&#x6570;&#x636E;&#x4FDD;&#x62A4;&#x6761;&#x4F8B;&#xFF0C;&#x7528;&#x6237;&#x6388;&#x6743;&#xFF0C;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x6570;&#x636E;</td>
      <td
      style="text-align:left">2.3.2 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">disable</td>
      <td style="text-align:left">GrowingIO &#x505C;&#x6B62;&#x91C7;&#x96C6;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">resume</td>
      <td style="text-align:left">GrowingIO &#x6062;&#x590D;&#x91C7;&#x96C6;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">stop</td>
      <td style="text-align:left">GrowingIO &#x505C;&#x6B62;&#x91C7;&#x96C6;&#xFF0C;&#x53EF;&#x4EE5;&#x4E0D;&#x5728;&#x4E3B;&#x7EBF;&#x7A0B;&#x8C03;&#x7528;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setThrottle</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x8282;&#x6D41;&#x53D1;&#x9001;&#xFF08;&#x8282;&#x6D41;&#x53D1;&#x9001;&#x65F6;imp&#x4E0D;&#x53D1;&#x9001;&#xFF09;&#xFF0C;&#x5185;&#x90E8;&#x5B9E;&#x9645;&#x8C03;&#x7528;
        Configuration &#x4E2D;&#x7684;&#x540C;&#x540D;&#x65B9;&#x6CD5;&#xFF0C;&#x6240;&#x4EE5;&#x5728;&#x521D;&#x59CB;&#x5316;&#x65F6;&#x5019;&#x914D;&#x7F6E;&#x548C;&#x8FD0;&#x884C;&#x65F6;&#x52A8;&#x6001;&#x914D;&#x7F6E;&#xFF0C;&#x6548;&#x679C;&#x4E00;&#x6837;&#x3002;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setImp</td>
      <td style="text-align:left"> <code>imp</code>&#x4E8B;&#x4EF6;&#x5F00;&#x5173;&#xFF0C;<code>true</code> &#x4E3A;&#x6253;&#x5F00;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">disableImpression</td>
      <td style="text-align:left">&#x4E0D;&#x53D1;&#x9001; <code>imp</code>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">ignoredView</td>
      <td style="text-align:left">
        <p>&#x5FFD;&#x7565;&#x914D;&#x7F6E;&#x7684; View &#xFF0C;&#x4E0D;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x6570;&#x636E;&#x3002;</p>
        <p>&#x5982;&#x679C;&#x60A8;&#x9700;&#x8981;&#x5FFD;&#x7565;&#x67D0;&#x4E9B;&#x7279;&#x6B8A;&#x5185;&#x5BB9;&#xFF0C;&#x6BD4;&#x5982;&#x5012;&#x8BA1;&#x65F6;&#x5143;&#x7D20;&#x6216;&#x6D89;&#x53CA;&#x9690;&#x79C1;&#x7684;&#x5185;&#x5BB9;&#xFF0C;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x6B64;&#x63A5;&#x53E3;&#x3002;</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">ignoreFragment</td>
      <td style="text-align:left">&#x4E0D;&#x91C7;&#x96C6;&#x914D;&#x7F6E;&#x7684; Fragment &#x9875;&#x9762;&#x6D4F;&#x89C8;&#x4E8B;&#x4EF6;&#xFF08;<code>page</code>&#xFF09;&#xFF0C;&#x4E0D;&#x5C06;<code>Fragment</code>&#x89C6;&#x4F5C;&#x4E00;&#x4E2A;&#x9875;&#x9762;&#xFF0C;&#x53EF;&#x4EE5;&#x7406;&#x89E3;&#x6210;&#x5F53;&#x4F5C;&#x4E3A;&#x4E00;&#x4E2A;&#x53EF;&#x70B9;&#x51FB;&#x7684;view&#x3002;&#x81EA;&#x52A8;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x884C;&#x4E3A;&#x4E8B;&#x4EF6;&#xFF08;<code>clck&#x3001;chng</code>&#xFF09;&#x548C;&#x5143;&#x7D20;&#x5C55;&#x793A;&#x4E8B;&#x4EF6;&#xFF08;<code>imp</code>&#xFF09;&#x3002;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">ignoreFragmentX</td>
      <td style="text-align:left">&#x652F;&#x6301; AndroidX &#xFF0C; &#x529F;&#x80FD;&#x540C; ignoreFragment&#x3002;</td>
      <td
      style="text-align:left">2.6.6 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">ignoreViewImp</td>
      <td style="text-align:left">
        <p>&#x5FFD;&#x7565;&#x914D;&#x7F6E;&#x7684; View &#xFF0C;&#x4E0D;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x5143;&#x7D20;&#x6D4F;&#x89C8;&#x6570;&#x636E;&#x3002;</p>
        <p>&#x5982;&#x679C;&#x60A8;&#x9700;&#x8981;&#x5FFD;&#x7565;&#x67D0;&#x4E9B;&#x5927;&#x91CF;&#x7684;
          &#x6570;&#x636E;&#xFF0C;&#x6BD4;&#x5982;&#x5F39;&#x5E55;&#xFF0C;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x6B64;&#x63A5;&#x53E3;&#x3002;</p>
      </td>
      <td style="text-align:left">2.6.7 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">setPageName</td>
      <td style="text-align:left">
        <p>&#x8BBE;&#x7F6E;&#x9875;&#x9762;&#x522B;&#x540D;&#xFF0C;&#x6709;&#x4E9B;&#x65F6;&#x5019;&#xFF0C;&#x5BF9;&#x4E8E;&#x5B8C;&#x6210;&#x67D0;&#x4E2A;&#x529F;&#x80FD;&#x7684;&#x9875;&#x9762;&#xFF0C;&#x7EDF;&#x8BA1;&#x65F6;&#x53EF;&#x80FD;&#x9700;&#x8981;&#x8FDB;&#x4E00;&#x6B65;&#x7EC6;&#x5206;&#x3002;
          &#x6BD4;&#x5982;&#xFF0C;&#x5BF9;&#x4E8E;&#x5C55;&#x793A;&#x5546;&#x54C1;&#x5217;&#x8868;&#x7684;&#x9875;&#x9762;&#xFF0C;&#x9700;&#x8981;&#x533A;&#x5206;&#x8863;&#x7269;&#x7C7B;&#x5546;&#x54C1;&#xFF0C;&#x4EE5;&#x53CA;&#x98DF;&#x54C1;&#x7C7B;&#x5546;&#x54C1;&#x7684;&#x4E24;&#x79CD;&#x5217;&#x8868;&#x7684;&#x8BBF;&#x95EE;&#x91CF;&#x3002;</p>
        <p></p>
        <p>&#x6CE8;&#x610F;</p>
        <ol>
          <li>&#x5FC5;&#x987B;&#x5728;&#x8BE5;<code>Activity</code>&#x7684;<code>onCreate</code>&#x65B9;&#x6CD5;&#x4E2D;&#x5B8C;&#x6210;&#x8BE5;&#x5C5E;&#x6027;&#x7684;&#x8D4B;&#x503C;&#x64CD;&#x4F5C;&#x3002;</li>
          <li>&#x9875;&#x9762;&#x522B;&#x540D;&#x53EA;&#x80FD;&#x8BBE;&#x7F6E;&#x4E3A;&#x5B57;&#x6BCD;&#x3001;&#x6570;&#x5B57;&#x548C;&#x4E0B;&#x5212;&#x7EBF;&#x7684;&#x7EC4;&#x5408;&#x3002;</li>
          <li>&#x4E3A;&#x67E5;&#x770B;&#x6570;&#x636E;&#x65B9;&#x4FBF;&#xFF0C;&#x8BF7;&#x5C3D;&#x91CF;&#x5BF9;iOS&#x548C;&#x5B89;&#x5353;&#x7684;&#x540C;&#x529F;&#x80FD;&#x9875;&#x9762;&#x53D6;&#x4E0D;&#x540C;&#x7684;&#x540D;&#x79F0;&#x3002;</li>
        </ol>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setPageNameX</td>
      <td style="text-align:left">&#x652F;&#x6301; AndroidX &#xFF0C; &#x529F;&#x80FD;&#x540C; setPageName&#x3002;</td>
      <td
      style="text-align:left">2.6.6 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">getSessionId</td>
      <td style="text-align:left">&#x5F97;&#x5230; session id</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">getDeviceId</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x8BBE;&#x5907;id&#xFF0C;&#x5BF9;&#x5E94;&#x6570;&#x636E;&#x91C7;&#x96C6;&#x7684;<code>u</code>&#x5B57;&#x6BB5;&#xFF0C;&#x53C8;&#x79F0;&#x4E3A;<code>&#x533F;&#x540D;&#x7528;&#x6237;id</code>&#xFF0C;&#x7528;&#x6765;&#x5B9A;&#x4E49;&#x4E00;&#x53F0;&#x8BBE;&#x5907;&#xFF0C;SDK
        &#x81EA;&#x52A8;&#x751F;&#x6210;&#x3002;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="android-sdk.md#cai-ji-guang-gao-banner-shu-ju">trackBanner</a>
      </td>
      <td style="text-align:left"><a href="android-sdk.md#cai-ji-guang-gao-banner-shu-ju">&#x8BBE;&#x7F6E;&#x6240;&#x6709;&#x5E7F;&#x544A;&#x56FE;&#x5BF9;&#x5E94;&#x7684;&#x5E7F;&#x544A;&#x5185;&#x5BB9;&#x63CF;&#x8FF0;&#xFF0C;&#x5185;&#x5BB9;&#x63CF;&#x8FF0;&#x9700;&#x8981;&#x8DDF;&#x5E7F;&#x544A;&#x7684;&#x987A;&#x5E8F;&#x76F8;&#x540C;&#x3002;</a>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&#x200B;<a href="android-sdk.md#cai-ji-shu-ru-kuang-shu-ju">trackEditText</a>
      </td>
      <td style="text-align:left">
        <p>&#x200B;<a href="android-sdk.md#cai-ji-shu-ru-kuang-shu-ju">SDK &#x9ED8;&#x8BA4;&#x4E0D;&#x91C7;&#x96C6;&#x7528;&#x6237;&#x8F93;&#x5165;&#x6846;&#x7684;&#x5185;&#x5BB9;&#xFF0C;&#x8BBE;&#x7F6E;&#x4EE5;&#x540E;&#xFF0C;&#x91C7;&#x96C6;&#x9664;&#x4E86;&#x5BC6;&#x7801;&#x4EE5;&#x5916;&#x7684;&#x8F93;&#x5165;&#x6846;&#x6587;&#x672C;&#x5185;&#x5BB9;&#x3002;&#x200B;</a>
        </p>
        <p>&#x5F53;&#x8FD9;&#x4E2A;&#x8F93;&#x5165;&#x6846;&#x5931;&#x53BB;&#x7126;&#x70B9;&#xFF08;&#x5305;&#x62EC;&#x5E94;&#x7528;&#x9000;&#x5230;&#x540E;&#x53F0;&#xFF09;&#xFF0C;&#x4E14;&#x8F93;&#x5165;&#x6846;&#x5185;&#x5BB9;&#x8DDF;&#x83B7;&#x53D6;&#x7126;&#x70B9;&#x524D;&#x76F8;&#x6BD4;&#x53D1;&#x751F;&#x53D8;&#x5316;&#x65F6;&#xFF0C;&#x8F93;&#x5165;&#x6846;&#x5185;&#x6587;&#x5B57;&#x4F1A;&#x88AB;&#x53D1;&#x9001;&#x56DE;GrowingIO&#x3002;</p>
        <p>&#x6CE8;&#x610F;&#xFF1A;&#x5BF9;&#x4E8E;&#x5BC6;&#x7801;&#x8F93;&#x5165;&#x6846;&#xFF0C;&#x5373;&#x4FBF;&#x6807;&#x8BB0;&#x4E3A;&#x9700;&#x8981;&#x91C7;&#x96C6;&#xFF0C;SDK&#x4E5F;&#x4F1A;&#x5FFD;&#x7565;&#xFF0C;&#x4E0D;&#x91C7;&#x96C6;&#x5B83;&#x7684;&#x6570;&#x636E;&#x3002;</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">trackFragment</td>
      <td style="text-align:left">
        <p>&#x5982;&#x679C;APP&#x521D;&#x59CB;&#x5316;&#x65F6;&#x5019;&#xFF0C;&#x6CA1;&#x6709;&#x8BBE;&#x7F6E; <code>trackAllFragment</code> &#x5373;&#x4E0D;&#x91C7;&#x96C6;&#x5168;&#x90E8; <code>Fragment</code>&#xFF0C;&#x53EF;&#x4EE5;&#x9009;&#x62E9;&#x6027;&#x91C7;&#x96C6;&#x6307;&#x5B9A; <code>Fragment</code>&#xFF0C;&#x8BBE;&#x7F6E;&#x4E4B;&#x540E;
          sdk &#x5C06;&#x76D1;&#x542C; <code>Fragment</code> &#x7684;&#x5404;&#x4E2A;&#x751F;&#x547D;&#x5468;&#x671F;&#xFF0C;
          &#x91C7;&#x96C6;&#x76F8;&#x5173;&#x7528;&#x6237;&#x884C;&#x4E3A;&#x6570;&#x636E;&#x3002;</p>
        <p>&#x8BF7;&#x5728; <code>new Fragment </code>&#x7684;&#x65F6;&#x5019;&#x8C03;&#x7528;&#x6B64;&#x65B9;&#x6CD5;&#x3002;</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">trackFragmentX</td>
      <td style="text-align:left">
        <p>&#x652F;&#x6301; AndroidX &#xFF0C; &#x529F;&#x80FD;&#x540C; trackFragment&#x3002;</p>
        <p>&#x8BF7;&#x5728; <code>new Fragment </code>&#x7684;&#x65F6;&#x5019;&#x8C03;&#x7528;&#x6B64;&#x65B9;&#x6CD5;&#x3002;</p>
      </td>
      <td style="text-align:left">2.6.6 &#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">trackWebView</td>
      <td style="text-align:left">&#x91C7;&#x96C6; <code>WebView</code> &#x4E8B;&#x4EF6;&#xFF0C;&#x9ED8;&#x8BA4;&#x91C7;&#x96C6;&#xFF0C;&#x60A8;&#x53EF;&#x4EE5;&#x5728;&#x4E0D;&#x5168;&#x91CF;&#x91C7;&#x96C6;<code>WebView</code>&#x7684;&#x65F6;&#x5019;&#xFF0C;&#x5B9A;&#x5236;&#x91C7;&#x96C6;&#x67D0;&#x4E2A;<code>WebView</code>
      </td>
      <td style="text-align:left">2.6.0 &#x4E2D;&#x5220;&#x9664;</td>
    </tr>
    <tr>
      <td style="text-align:left">trackX5WebView</td>
      <td style="text-align:left">&#x91C7;&#x96C6; X5WebView &#x4E8B;&#x4EF6;&#xFF0C;&#x9ED8;&#x8BA4;&#x91C7;&#x96C6;</td>
      <td
      style="text-align:left">2.6.0 &#x4E2D;&#x5220;&#x9664;</td>
    </tr>
    <tr>
      <td style="text-align:left">setTabName</td>
      <td style="text-align:left">&#x5982;&#x679C;&#x60A8;&#x6709;&#x67D0;&#x4E9B;View&#x52A8;&#x6001;&#x6DFB;&#x52A0;&#x5230;ViewTree&#x4E2D;&#x5E76;&#x4E14;&#x5728;&#x7236;&#x5BB9;&#x5668;&#x4E2D;&#x7684;&#x4F4D;&#x7F6E;&#x4E0D;&#x56FA;&#x5B9A;&#xFF08;&#x4F8B;&#x5982;&#x5E38;&#x89C1;&#x7684;&#x591A;Fragment&#x5B9E;&#x73B0;&#x7684;Tab&#x5207;&#x6362;&#xFF09;&#xFF0C;&#x8BF7;&#x7ED9;&#x6BCF;&#x4E2A;View&#x8BBE;&#x7F6E;ID&#x6765;&#x8F85;&#x52A9;&#x7EDF;&#x8BA1;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setImeiEnable</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>Android 2.7.8</b> &#x65B0;&#x589E;&#x8FD0;&#x884C;&#x65F6;&#x914D;&#x7F6E;&#x63A5;&#x53E3;&#xFF0C;&#x8BBE;&#x7F6E;&#x4E3A;
        false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; imei &#x3002;</td>
      <td style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">setAndroidIdEnable</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>Android 2.7.8</b> &#x65B0;&#x589E;&#x8FD0;&#x884C;&#x65F6;&#x914D;&#x7F6E;&#x63A5;&#x53E3;&#xFF0C;&#x8BBE;&#x7F6E;&#x4E3A;
        false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; <code>androidId</code> &#x3002;</td>
      <td
      style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">setGoogleAdIdEnable</td>
      <td style="text-align:left">&#x4E3A;&#x4E86;&#x6D77;&#x5916;&#x5E94;&#x7528;&#x5E02;&#x573A;&#x4E0A;&#x67B6;&#x5E94;&#x7528;&#xFF0C;<b>Android 2.7.8</b> &#x65B0;&#x589E;&#x8FD0;&#x884C;&#x65F6;&#x914D;&#x7F6E;&#x63A5;&#x53E3;&#xFF0C;&#x8BBE;&#x7F6E;&#x4E3A;
        false &#x5219; SDK &#x4E0D;&#x91C7;&#x96C6; <code>GoogleAdId</code> &#x3002;</td>
      <td
      style="text-align:left">2.7.8&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">setOAIDEnable</td>
      <td style="text-align:left">&#x56FD;&#x5185;<a href="http://www.msa-alliance.cn/col.jsp?id=120">&#x79FB;&#x52A8;&#x5B89;&#x5168;&#x8054;&#x76DF;MSA</a> &#x8054;&#x5408;&#x5404;&#x5927;&#x624B;&#x673A;&#x5236;&#x9020;&#x5546;&#x63A8;&#x51FA;&#x4E86;
        OAID &#xFF0C; &#x4F5C;&#x4E3A;&#x552F;&#x4E00;&#x5E7F;&#x544A;&#x6807;&#x8BC6;&#x7B26;&#x3002;<b>Android 2.8.5 </b>&#x65B0;&#x589E;&#x3002;</td>
      <td
      style="text-align:left">2.8.5&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
  </tbody>
</table>### 

### 自定义事件和变量API

在 Android SDK 文档中描述的更详细，请点击查看。

| 自定义事件和变量API | 说明 | 最低版本 |
| :--- | :--- | :--- |
| track | 发送自定义事件，对应`cstm`事件 | 2.0.0 |
| setPageVariable | 发送页面级变量，对应`pvar`事件 | 2.0.0 |
| setPageVariableX | 支持 AndroidX ， 功能同 setPageVariable | 2.6.6 以上 |
| setEvar | 发送转化变量，对应`evar`事件 | 2.0.0 |
| setPeopleVariable | 发送用户变量，对应`ppl`事件 | 2.0.0 |
| setUserId | 设置登录用户ID，对应`cs1`字段 | 2.0.0 |
| clearUserId | 清除登录用户ID | 2.0.0 |
| setVisitor | 设置访问用户变量，对应`vstr`事件 | 2.4.0 |

