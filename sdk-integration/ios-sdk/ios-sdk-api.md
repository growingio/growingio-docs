---
description: >-
  GrowingIO 提供了初始化配置项 API 和运行时 API 来自定义 SDK
  的采集，满足不同场景的定制采集。同一种含义的API，只需要调用一次即可，比如您关闭SDK的采集功能，可以在初始化中配置，也可以在运行时配置，只需调用一次相关API即可。
---

# iOS SDK API

{% hint style="warning" %}
注意：以下所有API说明为相应接口的使用方法和基本功能，具体的函数定义及详细说明请参考Growing.h。
{% endhint %}

### GrowingIO 初始化配置项 API

      GrowingIO 初始化配置项均在AppDelegate.m文件中的didFinishLaunchingWithOptions方法中 SDK 初始化代码块中设置，下面将分类并描述含义。

### 示例代码

```text
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    [Growing startWithAccountId:@"0a1b4118dd954ec3bcc69da5138bdb96"];
    //开启日志输出
    [Growing setEnableLog:YES];
    return YES；
}
```

###  基础配置 API

#### 1,初始化项目并设置采样率:\(startWithAccountId：withSampling）

      初始化方法，accountId 为项目ID，sampling 为采样率，例：若设置sampling = 0.01，则 1% 的设备会被采集数据,每次启动会根据用户设置的采样率判断设备是否在采集的范围之内。​

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| accountId | NSString | 是 | 项目ID |
| sampling | CGFloat | 否 | 采样率 |

原型：+ \(void\)startWithAccountId:\(NSString \*\)accountId withSampling:\(CGFloat\)sampling;  



#### 2，初始化项目:\(startWithAccountId\)

        初始化方法，accountId 为项目ID，默认采样率为 100%。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| accountId | NSString | 是 | 项目ID |

原型：+ \(void\)startWithAccountId:\(NSString \*\)accountId;

####  3，URL schemes 处理方法:（handlerUrl）

         URL schemes 处理方法，通过参数不同区分圈选、MobileDebugger、DeepLink等。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| url | NSString | 是 | URL schemes |

原型：+ \(BOOL\)handleUrl:\(NSURL\*\)url;       

###   SDK 功能API

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x914D;&#x7F6E;API</th>
      <th style="text-align:center">&#x9ED8;&#x8BA4;&#x503C;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">
        <p>&#x6700;&#x4F4E;</p>
        <p>&#x7248;&#x672C;</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">sdkVersion</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x5F53;&#x524D;GrowingIO SDK &#x7248;&#x672C;&#x53F7;</td>
      <td
      style="text-align:left">2.0.0</td>
    </tr>
    <tr>
      <td style="text-align:left">setEnableLog</td>
      <td style="text-align:center">YES</td>
      <td style="text-align:left">&#x8C03;&#x8BD5;&#x65E5;&#x5FD7;&#x5F00;&#x5173;&#xFF0C;enableLog == YES
        &#x65F6;&#xFF0C;&#x4F1A;&#x8F93;&#x51FA;&#x8C03;&#x8BD5;&#x65E5;&#x5FD7;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">getEnableLog</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x8C03;&#x8BD5;&#x65E5;&#x5FD7;&#x5F00;&#x5173;&#x7684;&#x5F53;&#x524D;&#x72B6;&#x6001;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>setDeviceIDModeTo</p>
        <p>CustomBlock</p>
      </td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">
        <p>&#x7528;&#x6237;&#x81EA;&#x5B9A;&#x4E49;ID&#xFF08;&#x5373; u &#x503C;&#xFF09;,&#x4F1A;&#x8986;&#x76D6;&#x539F;&#x6765;</p>
        <p>&#x7684; u &#x503C;</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setTrackerHost</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x6536;&#x96C6;&#x5E73;&#x53F0;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setReportHost</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x8BBE;&#x5907;&#x62A5;&#x6D3B;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setDataHost</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x67E5;&#x770B;&#x5E73;&#x53F0;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setGtaHost</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x540E;&#x53F0;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setWsHost</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x540E;&#x53F0;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>setHybridJSSDK</p>
        <p>UrlPrefix</p>
      </td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x540E;&#x53F0;&#x670D;&#x52A1;&#x5668;&#x5730;&#x5740;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">setZone</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E; zone &#x4FE1;&#x606F;&#xFF0C;&#x5373;&#x65F6;&#x533A;&#x4FE1;&#x606F;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">getDeviceId</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x5F53;&#x524D;&#x8BBE;&#x5907; id</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">getVisitUserId</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x5F53;&#x524D; uid</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">getSessionId</td>
      <td style="text-align:center">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x5F53;&#x524D;&#x8BBF;&#x95EE; id</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">growingAttributesDonotTrackImp</td>
      <td style="text-align:center">False</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x662F;&#x5426;&#x91C7;&#x96C6;view&#x53CA;&#x9875;&#x9762;&#x5143;&#x7D20;&#x7684;imp&#x4E8B;&#x4EF6;</td>
      <td
      style="text-align:left">2.6.7</td>
    </tr>
  </tbody>
</table>### 

### 数据采集发送 API

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x6570;&#x636E;&#x76F8;&#x5173;API</th>
      <th style="text-align:left">&#x9ED8;&#x8BA4;&#x503C;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">setAspectMode</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6570;&#x636E;&#x91C7;&#x96C6;&#x6A21;&#x5F0F;&#xFF0C;&#x6709;
        GrowingAspectModeSubClass &#x548C; GrowingAspectModeDynamicSwizzling &#x4E24;&#x79CD;</td>
    </tr>
    <tr>
      <td style="text-align:left">setEnableDiagnose</td>
      <td style="text-align:left">enable</td>
      <td style="text-align:left">
        <p>&#x662F;&#x5426;&#x5141;&#x8BB8;&#x53D1;&#x9001;&#x57FA;&#x672C;&#x6027;&#x80FD;&#x8BCA;&#x65AD;&#x4FE1;&#x606F;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E3A;&#x5F00;&#x3002;</p>
        <p>&#x57FA;&#x672C;&#x6027;&#x80FD;&#x6307;&#x53D1;&#x9001;&#x6210;&#x529F;&#x3001;&#x5931;&#x8D25;&#x3001;timeout&#x7B49;&#x4FE1;&#x606F;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">disable</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x5168;&#x5C40;&#x4E0D;&#x53D1;&#x9001;&#x7EDF;&#x8BA1;&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">enableAllWebViews</td>
      <td style="text-align:left">enable</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x662F;&#x5426;&#x91C7;&#x96C6; UIWebView / WKWebView
        &#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">enableHybridHashTag</td>
      <td style="text-align:left">enable</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x542F;&#x7528; HashTag</td>
    </tr>
    <tr>
      <td style="text-align:left">isTrackingWebView</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x542F;&#x7528; trackingWebView</td>
    </tr>
    <tr>
      <td style="text-align:left">setImp</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x662F;&#x5426;&#x53D1;&#x9001;&#x5143;&#x7D20;&#x7684;&#x5C55;&#x73B0;&#x6B21;&#x6570;&#xFF08;&#x6D4F;&#x89C8;&#x91CF;&#x3001;&#x66DD;&#x5149;&#x91CF;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">setFlushInterval</td>
      <td style="text-align:left">10s</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x3001;&#x83B7;&#x53D6;&#x53D1;&#x9001;&#x6570;&#x636E;&#x7684;&#x65F6;&#x95F4;&#x95F4;&#x9694;&#xFF0C;&#x9ED8;&#x8BA4;&#x503C;&#x4E3A;10&#x79D2;</td>
    </tr>
    <tr>
      <td style="text-align:left">setDailyDataLimit</td>
      <td style="text-align:left">3M</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6BCF;&#x5929;&#x4F7F;&#x7528;&#x6570;&#x636E;&#x7F51;&#x7EDC;&#xFF08;2G&#x3001;3G&#x3001;4G&#xFF09;&#x4E0A;&#x4F20;&#x7684;&#x6570;&#x636E;&#x91CF;&#x7684;&#x4E0A;&#x9650;&#xFF08;&#x5355;&#x4F4D;&#x662F;
        KB&#xFF09;&#xFF0C;&#x9ED8;&#x8BA4;&#x503C;&#x4E3A; 3 MB</td>
    </tr>
    <tr>
      <td style="text-align:left">getDailyDataLimit</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x6BCF;&#x5929;&#x4F7F;&#x7528;&#x6570;&#x636E;&#x7F51;&#x7EDC;&#xFF08;2G&#x3001;3G&#x3001;4G&#xFF09;&#x4E0A;&#x4F20;&#x7684;&#x6570;&#x636E;&#x91CF;&#x7684;&#x4E0A;&#x9650;&#xFF08;&#x5355;&#x4F4D;&#x662F;
        KB&#xFF09;&#xFF0C;&#x9ED8;&#x8BA4;&#x503C;&#x4E3A;3 MB</td>
    </tr>
    <tr>
      <td style="text-align:left">disableDataCollect</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E; GDPR &#x751F;&#x6548;</td>
    </tr>
    <tr>
      <td style="text-align:left">enableDataCollect</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E; GDPR &#x5931;&#x6548;</td>
    </tr>
    <tr>
      <td style="text-align:left">disablePushTrack</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x662F;&#x5426;&#x91C7;&#x96C6;push&#x63A8;&#x9001;&#x70B9;&#x51FB;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E0D;&#x91C7;&#x96C6;</td>
    </tr>
  </tbody>
</table>### ​ 自定义事件和变量API 

在 IOS SDK 文档中描述的更详细，请[点击查看](https://docs.growingio.com/docs/sdk-integration/ios-sdk#zi-ding-yi-shi-jian-he-bian-liang-api)。

| API函数 | 说明 | 最低版本 |
| :--- | :--- | :--- |
| track | 发送自定义事件，对应cstm事件 | 2.0.0 |
| setPageVariable | 发送页面级变量，对应pvar事件 | 2.0.0 |
| setEvar | 发送转化变量，对应evar事件 | 2.0.0 |
| setPeopleVariable | 发送用户变量，对应ppl事件 | 2.0.0 |
| setUserId | 置登录用户ID，对应cs1字段 | 2.0.0 |
| clearUserId | 清除登录用户ID | 2.0.0 |
| setVisitor | 设置访问用户变量，对应vstr事件 | 2.4.0 |

###  2.X 动态添加属性说明

####  （1）UIView 增加属性  

```text
// 手动标识该view不要追踪
@property (nonatomic, assign) BOOL growingAttributesDonotTrack; 

// 手动标识该view不要追踪它的值，默认是NO，特别的UITextView，UITextField，
//UISearchBar默认是YES
@property (nonatomic, assign) BOOL growingAttributesDonotTrackValue; 

//手动标识该view的取值  比如banner广告条的id 可以放在banner按钮的任意view上
@property (nonatomic, copy)   NSString *growingAttributesValue; 

// 手动标识SDCycleScrollView组件的bannerIds  如若使用,请在创建SDCycleScrollView实例对象后,
//立即赋值;(如果不进行手动设置,SDK默认会采集banner的imageName或者imageURL)
@property (nonatomic, strong)  NSArray<NSString *> *growingSDCycleBannerIds;

 // 手动标识该view的附加属性  该值可被子节点继承
@property (nonatomic, copy)   NSString* growingAttributesInfo; 
```

####  （2）UIViewController 增加属性 

```text
// 手动标识该vc的附加属性  该值可被子节点继承
@property (nonatomic, copy)   NSString* growingAttributesInfo; 

// 手动标识该页面的标题，必须在该UIViewController显示之前设置
@property (nonatomic, copy)   NSString* growingAttributesPageName; 
```

