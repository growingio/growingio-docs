---
description: >-
  GrowingIO 提供了初始化配置项 API 和运行时 API 来自定义 SDK
  的采集，满足不同场景的定制采集。同一种含义的API，只需要调用一次即可，比如您关闭SDK的采集功能，可以在初始化中配置，也可以在运行时配置，只需调用一次相关API即可。
---

# Android API

## GrowingIO 初始化配置项 API

GrowingIO 初始化配置项均在`Application`的`onCreate`方法中 SDK 初始化代码块中设置，下面将分类并描述含义。

### 示例代码

```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        GrowingIO.startWithConfiguration(this,new Configuration()
            .disableCellularImp()
            .disableImageViewCollection(false)
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
    }
}
```



### 基础配置 API

| 初始化配置项API                                                         | 默认值                           | 说明 | 最低版本                                 |
| :--- | :--- | :--- | :--- |
| setTestMode | false | 在Logcat中输出采集日志 |  |
| setDebugMode | false | 实时发送数据，开启则不遵循移动网络状态下数据发送大小默认 3M 限制以及采集数据缓存30秒发送策略。为了方便开发者查看日志，一般和`setTestMode`一起使用。 |  |
| setChannel | 无 | 设置渠道 |  |
| useID | true | 是否在计算`xpath`时使用控件`id，`默认使用 | 2.5.0中删除 |



### SDK 功能API

| 初始化配置项API | 默认值                    | 说明 | 最低版本                    |
| :--- | :--- | :--- | :--- |
| setDeeplinkCallback | 无 | DeepLink 回调接口，获得自定义参数以便跳转对应 APP页 面 | 2.3.2 |
| setTrackWebView | true | 设置为`false`时不采`WebView`数据 |  |
| supportMultiProcessCircle | false | 是否使用多进程圈选功能 |  |
| setMutiprocess | false | 使用了多进程必须配置，自定义事件和变量值才会多进程共享 |  |
| trackAllFragments | false | 是否采集所有Fragment |  |
| setHashTagEnable | false | 在`WebView`中的页面访问，是否认为点击锚点链接是一个页面浏览 |  |
| setTrackWebView | true | 是否采集全部的`WebView` |  |



### 数据采集发送 API

| 初始化配置项API | 默认值                                                        | 说明 |
| :--- | :--- | :--- |
| setDisabled | false | SDK 是否采集数据，设置为`true`时不采集数据 |
| setSampling | 1 | 采样率\[0.01~1\],若设置sampling = 0.01，则 1% 的设备会被采集数据，每次启动会根据用户设置的采样率判断设备是否在采集的范围之内，使用**之前请咨询技术支持** |
| setSessionInterval | 30 \* 1000 | 在后台停留时长超过此值，则产生新的`sessionId`,发送`visit`事件。 |
| setFlushInterval | 30 \* 1000 | 数据刷新的最长时间间隔，默认30 秒 。如果距离上次发送数据事件超过此时间则发送事件 |
| setThrottle | false | 是否节流发送，节流发送时`imp`不发送，不发送但是采集，`imp`为元素展示事件 |
| setDisableImpression | false | 是否采集`imp`事件，`imp`为元素展示事件，默认采集`imp` |
| disableCellularImp | false | 否关闭移动蜂窝网`imp`事件采集，`imp`为元素展示事件 |
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
| setChannel | 设置渠道名称 |
| setGeoLocation | 设置经纬度，并在 vst 事件中发送，Android SDK 暂时没办法自动获取`GPS`数据，如果您要采集`GPS`数据，需要在您的App每次获取完`GPS`数据之后，通过该`API`告知 SDK。如果您不设置，我们默认使用用户`IP`的`Location。` |
| clearGeoLocation | 清空经纬度 |
| setViewInfo | 配置 view 的 Tag，标记 View ，并在 GrowingIO 相关事件中发送 ，内容对应 `xPath` 中的 `obj`例如：在商品`ListView`添加购物车的场景中，每个商品`item`都含有一个加入购物车按钮，这时无法区分商品`item`与将它加入购物车按钮的一对一关系，此时调用此方法增加描述。注意：适用于原有`v`字段含义不大，只关注描述的场景，使用此接口后`v`字段将不采集 |
| setViewContent | 配置 view 的 Tag，标记 View ，并在 GrowingIO相关事件中发送，内容对应 `xPath` 中的 `v`SDK默认不会采集ImageView的内容，为了能对不同的图片元素（ImageView）区分统计，需要对每个具有分析意义的图片元素（ImageView）添加描述。 |
| setViewID | 设置 View id ，配置之后对应 xPath 中的 view id，SDK将会使用Layout文件中的ID来识别一个元素。如果部分元素在Layout文件中没有ID，建议在Layout文件中添加。对于动态生成的元素，可以使用如下方法对它设置唯一的ID。当您的应用界面改版时，可能会导致无法准确地统计已经圈选的元素。因此，对于应用中的主要流程涉及到的界面元素，建议您为它们设置固定的唯一ID，以保证数据的一致性。 |



### 数据采集 API 

| 运行时API | 说明 | 最低版本 |
| :--- | :--- | :--- |
| disableDataCollect | 遵守欧洲联盟出台的通用数据保护条例，用户不授权，不采集用户数据 | 2.3.2 |
| enableDataCollect | 遵守欧洲联盟出台的通用数据保护条例，用户授权，采集用户数据 | 2.3.2 |
| disable | GrowingIO 停止采集 |  |
| resume | GrowingIO 恢复采集 |  |
| stop | GrowingIO 停止采集，可以不在主线程调用 |  |
| setThrottle | 是否节流发送（节流发送时imp不发送），内部实际调用 Configuration 中的同名方法，所以在初始化时候配置和运行时动态配置，效果一样。 |  |
| setImp |  `imp`事件开关，`true` 为打开 |  |
| disableImpression | 不发送 `imp` |  |
| ignoredView | 忽略配置的 View ，不采集用户数据。如果您需要忽略某些特殊内容，比如倒计时元素或涉及隐私的内容，可以使用该功能。 |  |
| ignoreFragment | 忽略配置的 Fragment ，不采集用户数据。如果您需要忽略某些特殊内容，比如倒计时元素或涉及隐私的内容，可以使用该功能。 |  |
| setPageName | 设置页面别名，有些时候，对于完成某个功能的页面，统计时可能需要进一步细分。 比如，对于展示商品列表的页面，需要区分衣物类商品，以及食品类商品的两种列表的访问量。注意 |  |
| getSessionId | 得到 session id |  |
| [trackBanner](./#cai-ji-guang-gao-banner-shu-ju) | [设置所有广告图对应的广告内容描述，内容描述需要跟广告的顺序相同。](./#cai-ji-guang-gao-banner-shu-ju) |  |
| ​[trackEditText](./#cai-ji-shu-ru-kuang-shu-ju) | ​[SDK 默认不采集用户输入框的内容，设置以后，采集除了密码以外的输入框文本内容。​](./#cai-ji-shu-ru-kuang-shu-ju)当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。 |  |
| trackFragment | 如果APP初始化时候，没有设置 `trackAllFragment` 即不采集全部 `Fragment`，可以选择性采集指定 `Fragment`，设置之后 sdk 将监听 `Fragment` 的各个生命周期， 采集相关用户行为数据。 |  |
| trackWebView | 采集 `WebView` 事件，默认采集，您可以在不全量采集`WebView`的时候，定制采集某个`WebView` |  |
| trackX5WebView | 采集 X5WebView 事件，默认采集 |  |
| setTabName | 如果您有某些View动态添加到ViewTree中并且在父容器中的位置不固定（例如常见的多Fragment实现的Tab切换），请给每个View设置ID来辅助统计 |  |

### 

### 自定义事件和变量API

在 Android SDK 文档中描述的更详细，请点击查看。

| 自定义事件和变量API | 说明 | 最低版本 |
| :--- | :--- | :--- |
| track | 发送自定义事件，对应`cstm`事件 | 2.0.0 |
| setPageVariable | 发送页面级变量，对应`pvar`事件 | 2.0.0 |
| setEvar | 发送转化变量，对应`evar`事件 | 2.0.0 |
| setPeopleVariable | 发送用户变量，对应`ppl`事件 | 2.0.0 |
| setUserId | 置登录用户ID，对应cs1字段 | 2.0.0 |
| clearUserId | 清除登录用户ID | 2.0.0 |
| setVisitor | 设置访问用户变量，对应`vstr`事件 | 2.4.0 |

