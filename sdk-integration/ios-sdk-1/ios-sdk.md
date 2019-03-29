---
description: GrowingIO 无埋点 SDK 会自动采集用户行为数据、页面元素信息、设备信息等数据。
---

# iOS 无埋点SDK

## **组件化SDK**

GrowingIO iOS SDK2.7.4 包含以下2个组件SDK:

•  GrowingCoreKit \(组件基础库,具备分析功能\)

•  GrowingAutoTrackKit \(无埋点库\)

**集成环境**

•  Xcode9.0或更高版本

•   iOS 8及以上

## 无埋点 SDK集成 

在您的 iOS  项目中集成 GrowingIO SDK，使用 GrowingIO 提供的多种工具来分析用户行为。

### 1. 选择集成方式

#### （1）使用 CocoaPods 快速集成

* 添加`pod 'GrowingAutoTrackKit'`到 Podfile 中
* 执行`pod update`，不要用`--no-repo-update`选项
* 直接进行第 2 步 [“设置 URL Scheme”](ios-sdk.md#2-she-zhi-url-scheme)

#### （2）手动集成 SDK 

* 下载 iOS SDK 以下包：[GrowingHeader ](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-PublicHeader-2.7.3.zip)，[GrowingCoreKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.7.4.zip)，[GrowingAutoTrackKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-AutoTrackKit-2.7.4.zip)
* 解压 iOS SDK 压缩文件
*  将`Growing.h`，`GrowingCoreKit.framework`，`GrowingAutoTrackKit.framework`添加到iOS工程中。

{% hint style="warning" %}
#### **提醒:**  记得勾选 "Copy items if needed"
{% endhint %}

* 在工程项目中添加以下库文件

| 库名称 | 说明 |
| :--- | :--- |
| Foundation.framework | 基础依赖库 |
| Security.framework | 用于APP连接圈选页面SSL连接 |
| CoreTelephony.framework | 用于读取运营商名称 |
| SystemConfiguration.framework | 用于判断网络状态 |
| AdSupport.framework | 用于来源管理激活匹配 |
| libicucore.tbd | 用于APP连接圈选页面解析 |
| libsqlite3.tbd | 存储日志 |
| CoreLocation.framework | 用于读取地理位置信息（如果您的app有权限） |
| JavaScriptCore.framework | Web圈App交互 |
| WebKit.framework | Web圈选 |

{% hint style="warning" %}
#### 提醒：添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries
{% endhint %}

* 添加编译参数，并注意大小写：

![](../../.gitbook/assets/image%20%28172%29.png)

### 2. 设置 URL Scheme

####    2.1 获取 URL Scheme

* 添加新产品：登录官网 -&gt; 点击项目选择框  -&gt; 点击“设置”icon -&gt; 点击“新建应用”  -&gt; 选择添加 iOS 应用 -&gt; 填写“应用名称”，点击下一步 -&gt; 在第二段中标黄字体。
* 现有产品：登录官网  -&gt;   点击“设置”icon  -&gt;  点击“应用管理”  -&gt;  找到对应产品的 URL Scheme

![&#x5E94;&#x7528;&#x7BA1;&#x7406;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%28134%29.png)

####    2**.2  添加 URL Scheme（growing.xxxxxxxxxxxxxxxx）到项目中，以便唤醒您的程序进行圈选**

####    2**.3  在 AppDelegate 中添加激活圈选的代码**

```objectivec
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    if ([Growing handleUrl:url]) {
        return YES;
    }
    ...
    return NO;
}
```

{% hint style="warning" %}
### 提醒：

* 若您在 AppDelegate 中实现了以下一个或多个方法，请在已实现的函数中，调用`[Growing handleUrl:]`

  ```objectivec
  - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
  - (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
  - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
  ```

* 若以上所有方法均未实现，请实现以下方法并调用`[Growing handleUrl:]`

  ```objectivec
  - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
  ```

* 实际情况可能很复杂，请在调试时确保函数`[Growing handleUrl:]`会被执行到
{% endhint %}

### 3. 初始化

在 AppDelegate 中引入`#import "Growing.h"`并添加初始化方法

您的项目ID查看方式为：点击“设置”icon-&gt;点击“项目配置”

![&#x9879;&#x76EE;&#x7BA1;&#x7406;&#x9875;&#x9762;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2816%29.png)

![&#x9879;&#x76EE;ID&#x67E5;&#x770B;](../../.gitbook/assets/image%20%2878%29.png)

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    // 启动GrowingIO
    [Growing startWithAccountId:@"xxxxxxxxxxxxxxxx"]; //替换为您的项目ID
    // 其他配置
    // 开启Growing调试日志 可以开启日志
    // [Growing setEnableLog:YES];
}
```

{% hint style="warning" %}
#### 提醒：请确保将代码添加在上述位置，添加到其他方法或异步block中可能导致数据不准确！
{% endhint %}

至此，您的SDK安装成功了！

## 重要配置

{% hint style="info" %}
下列内容为常用配置，更多属性及接口详细信息见 Growing.h 
{% endhint %}

### 设置元素内容

      当您想采集一些可能没有文字的控件（比如UIImageView，UIView）时，也可以给属性growingAttributesValue 赋值作为文字，用来在圈选的时候区分不同的内容。

       如果您的 app 上方有横向滚动的 Banner 广告，若要收集 Banner 相关数据，请在响应点击的控件上添加如下代码：

```objectivec
UIView *view;
…
view.growingAttributesValue = 广告的唯一ID;
```

其中 view 是您的广告元素，请确保两点：

* 对不同广告图，广告的唯一 ID 也不相同
* 响应点击的控件，与设置 ID 的控件是同一个

#### 【例子】当您的横向滚动广告共有3张广告图时，您可以在3个响应点击的View上分别设置不同的广告唯一ID，实现方式：

```objectivec
view1.growingAttributesValue = @"ad1";
view2.growingAttributesValue = @"ad2";
view3.growingAttributesValue = @"ad3";
```





### 采集输入框数据

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置：

```objectivec
UIView *view; // view 可以是 UITextField, UITextView, UISearchBar
 ...
view.growingAttributesDonotTrackValue = NO;
```

view代表要被采集的输入框。 当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。

{% hint style="warning" %}
#### 提醒：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据
{% endhint %}



### Facebook广告SDK

如果您使用了 Facebook 广告 SDK，请务必在 main 函数第一行调用以下代码来避免冲突，否则可能造成无法创建项目或者统计准确性问题。注意：APP启动后，将不允许修改采集模式。

```objectivec
[Growing setAspectMode:GrowingAspectModeDynamicSwizzling];
```

### 采集 WebView 页面数据

SDK 会自动采集H5页面的数据，不需要特殊配置。

### 采集GPS数据

如果您的应用有相应权限，SDK 将自动采集您的GPS数据。

### 启用Hashtag识别

您可以在项目中添加以下方法以启用 Hashtag 识别：

```objectivec
// 设置为 YES, 将启用 HashTag
+ (void)enableHybridHashTag:(BOOL)enable;
```

### GDPR 数据采集开关

GrowingIO SDK  针对欧盟区的一般数据保护法\(GDPR\)提供了以下的API供开发者调用，`SDK 2.3.2`以上版本支持。

```objectivec
// 开启GDPR，不采集数据
[Growing disableDataCollect];
 
// 关闭GDPR，采集数据
[Growing enableDataCollect];
```



### Deep Link & Universal Link <a id="deeplink-hui-tiao-can-shu-huo-qu"></a>

| Deep Link 功能 | SDK版本 |
| :--- | :--- |
| 基础 Deep Link 功能（Scheme打开APP至首页） | 2.3.0 |
| 直达落地页（Scheme打开至活动页） | 2.3.2 |
| Universal Link 、应用宝微下载链接支持 | 2.4.1 |

#### 一、 Deep Link 

1. [确认 URL Scheme 添加正确](https://growingio.gitbook.io/docs/~/edit/drafts/-LOIW-mWXk4nUqRlFsYV/sdk-integration/ios-sdk#2-she-zhi-url-scheme)​。
2. 添加自定义参数回调方法：

```objectivec
// params 为解析正确时反回调的参数, error 为解析错误时返回的参数.
// handler 默认为空, 客户需要手动设置.
+ (void)registerDeeplinkHandler:(void(^)(NSDictionary *params, NSError *error))handler;
```

```objectivec
//DeepLink 调用示例
[Growing registerDeeplinkHandler:^(NSDictionary *params, NSError *error) {
        NSLog(@"params : %@", params);
        XCTAssertNotNil(params);
        XCTAssertEqualObjects(params[@"key1"], @"value1");
        XCTAssertEqualObjects(params[@"key2"], @"value2");
 }];
```

{% hint style="info" %}
* params 参数为您在 DeepLink 页面设置的“直达落地页参数”
* 请在 `+ (BOOL)handleUrl:(NSURL*)url`被调用前注册回调方法
{% endhint %}

#### **二、  Universal Link**

使用 Universal Link 唤醒APP， 步骤如下：

1.配置链接：[配置 Universal Link 、应用宝微链接（可选项）](../../configuration/project-configuration.md#4-1-pei-zhi-universal-link-ying-yong-bao-wei-lian-jie)。

2.请在`AppDelegate.m`添加以下代码：

```objectivec
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray * _Nullable))restorationHandler{
    [Growing handleUrl:userActivity.webpageURL];
	return YES;
}
```

3.添加 Universal Link 参数解析回调方法，此方法与 Deep Link 方法一致。

### 设置界面元素 ID

当您的应用界面改版时，可能会导致无法准确地统计已经圈选的元素。因此，对于应用中的主要流程涉及到的界面元素，建议您为它们设置固定的唯一ID，以保证数据的一致性。

{% hint style="warning" %}
* 主要流程是指登录、注册、购买、发帖等操作步骤
* 被设置 ID 的对象是界面的重要元素，如“注册”、“结算”、“发布”等按钮
{% endhint %}

若要为元素设置 ID，请在 viewWillAppear 或者时机更早的方法里添加以下代码：

```objectivec
-(void)viewWillAppear {
    UIView *MyView;
    …
    MyView.growingAttributesUniqueTag = @"my_view";
}
```

{% hint style="danger" %}
* ID 只能设置为字母、数字和下划线的组合
* 对于已经集成过旧版SDK并圈选过的应用，对某个元素设置ID后再圈选它，指标数值会从零开始计算，类似初次集成SDK后发版的效果，但不影响之前圈选的其它指标数据。如果不希望出现这种情况，请不要使用这个方法
{% endhint %}



### 在 App Store 提交应用

集成了 GrowingIO SDK 以后，默认会启用 IDFA，所以在向 App Store 提交应用时，需要：

* 对于问题 **Does this app use the Advertising Identifier \(IDFA\)**，选择 YES。
* 对于选项**Attribute this app installation to a previously served advertisement**，打勾。
* 对于选项**Attribute an action taken within this app to a previously served advertisement**，打勾。

{% hint style="info" %}
#### 为**什么 GrowingIO 使用 IDFA?**

GrowingIO 使用 IDFA 来做来源管理激活设备的精确匹配，让你更好的衡量广告效果。如果你不希望跟踪这个信息，可以选择不引入 AdSupport.framework 或者在用 Cocoapods 安装时使用 ‘GrowingIO/without-IDFA' subspec.
{% endhint %}

### 采集推送

在**IOS  SDK 2.6.3** 版本， 支持采集用户**点击**本应用的通知的题和内容，和 Android SDK 不同，**不支持通知展现事件的采集**。此功能默认关闭，如需开启，请在 Application 初始化 GrowingIO 中设置，例如：

```objectivec
- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
 ...
  //开启推送点击采集
  [Growing disablePushTrack:NO];
  ...
  }
  
```

**查看通知采集数据**

支持对于通知的展现和点击事件的采集，GrowingIO 并未增加新的采集事件类型，而是使用了自定义事件发送，所以需要您创建自定义事件和事件级变量，事件级变量标识符为**`notification_title`**，**`notification_content`**，自定义事件的标识符为**`notification_show`**，**`notification_click`**如图：

{% hint style="info" %}
iOS SDK 不支持通知展现的事件采集，但是 Android SDK 支持，这里为了满足大部分客户都有 Android 和 iOS 两个应用的情况，演示创建 `notificaion_show` 。
{% endhint %}

![&#x521B;&#x5EFA;&#x901A;&#x77E5;&#x7684;&#x4E8B;&#x4EF6;&#x7EA7;&#x53D8;&#x91CF;](../../.gitbook/assets/image%20%2866%29.png)

![&#x521B;&#x5EFA;&#x901A;&#x77E5;&#x7684;&#x81EA;&#x5B9A;&#x4E49;&#x4E8B;&#x4EF6;](../../.gitbook/assets/image%20%2874%29.png)

然后创建事件分析，等候片刻即可看到数据

![&#x521B;&#x5EFA;&#x63A8;&#x9001;&#x4E8B;&#x4EF6;&#x5206;&#x6790;](../../.gitbook/assets/image%20%28234%29.png)



## 自定义事件和变量 API 

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](../../data-definition/custom-event/)和[变量](../../data-definition/custom-event/event-variable.md)，下面介绍自定义事件和变量 API 使用方法。

SDK 提供多种不同类型的API，请根据您的实际需要正确地调用。

```objectivec
// 发送自定义事件 API
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;

// 发送页面级变量 API
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;

// 发送转化变量 API
+ (void)setEvarWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setEvarWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setEvar:(NSDictionary<NSString *, NSObject *> *)variable;

// 发送用户变量 API
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable;

// 访问用户变量 API
+ (void)setVisitor:(NSDictionary<NSString *, NSObject *> *)variable;

// 设置登录用户ID API
+ (void)setUserId:(NSString *)userId;

// 清除登录用户ID API
+ (void)clearUserId;
```

### track

发送一个自定义事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面声明事件以及事件级变量。

#### 参数说明：

| 参数名称 | 参数类型                                   | 是否必须                      | 说明 |
| :--- | :--- | :--- | :--- |
| eventId | String |       是 | 事件标识符 |
| number | Number |       否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariable | JSON Object |       否 | 事件发生时所伴随的维度信息。 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">&#x82F1;&#x6587;&#x6570;&#x5B57;&#x7EC4;&#x5408;&#x7684;&#x5B57;&#x7B26;&#x4E32;&#xFF0C;&#x4E0D;&#x80FD;&#x4E3A;
        nil &#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x957F;&#x5EA6;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;&#x4E14;&#x4E0D;&#x80FD;&#x542B;&#x6709;&#x7279;&#x6B8A;&#x5B57;&#x7B26;</td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">&#x6B63;&#x6574;&#x6570;&#x6216;&#x6D6E;&#x70B9;&#x6570;</td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x80FD;&#x4E3A; nil&#xFF1B;<code>eventLevelVariable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// track API原型
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// track API调用示例一
[Growing track:@"registerSuccess"];
```

```objectivec
// track API调用示例二
[Growing track:@"registerSuccess" withVariable:@{@"gender":@"male", @"age":@"21"}];
```

```objectivec
// track API调用示例三
[Growing track:@"loanAmount" withNumber:@800000 andVariable:@{@"loanType":@"houseMortgage", @"province":@"Zhejiang"}];
```

### setPageVariable

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

{% hint style="danger" %}
**SDK 2.6.7** 将页面级变量**`pageLevelVariables`**与该页面对象绑定，设置不同的值将会合并，如果想要清空，需要传 null 。
{% endhint %}

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 页面级别的信息 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">&#x4E0D;&#x80FD;&#x4E3A; nil &#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x957F;&#x5EA6;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">&#x4E0D;&#x80FD;&#x4E3A; nil &#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x82E5;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x5219;&#x957F;&#x5EA6;&#x5E94;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;
        1000</td>
    </tr>
    <tr>
      <td style="text-align:left">pageLevelVariable</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x80FD;&#x4E3A; nil&#xFF1B;<code>pageLevelVariable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setPageVariable API原型
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
```

```objectivec
// setPageVariable API调用示例一
[Growing setPageVariableWithKey:@"author" andStringValue:@"Zhang San" toViewController:myViewController];
```

```objectivec
// setPageVariable API调用示例二
[Growing setPageVariable:@{@"pageName":@"Home Page", @"author":@"Zhang San"} toViewController:myViewController];
```

### setEvar

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 转化变量用于高级归因分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">&#x4E0D;&#x80FD;&#x4E3A; nil &#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x957F;&#x5EA6;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50</td>
    </tr>
    <tr>
      <td style="text-align:left">Value</td>
      <td style="text-align:left">&#x53D8;&#x91CF;&#x4E0D;&#x4E3A;nil&#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x82E5;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x5219;&#x957F;&#x5EA6;&#x5E94;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;
        1000</td>
    </tr>
    <tr>
      <td style="text-align:left">conversionLevelVariable</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x80FD;&#x4E3A;nil&#xFF1B;<code>conversinoLevelVariable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setEvar API原型
+ (void)setEvarWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setEvarWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setEvar:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// setEvar API调用示例一
[Growing setEvarWithKey:@"campaignId" andStringValue:@"1234567890"];
```

```objectivec
// setEvar API调用示例二
[Growing setEvar:@{@"campaignId":@"12345", @"campaignOwner":@"Li Si"}];
```

### setPeopleVariable

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明用户变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 用户变量用于用户信息相关的分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">&#x4E0D;&#x80FD;&#x4E3A;nil&#x6216;&quot;&quot;&#xFF0C;&#x957F;&#x5EA6;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">&#x53D8;&#x91CF;&#x4E0D;&#x4E3A;nil&#x6216;&#x8005;&quot;&quot;&#xFF0C;&#x82E5;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x5219;&#x957F;&#x5EA6;&#x5E94;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;
        1000</td>
    </tr>
    <tr>
      <td style="text-align:left">customerVariables</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x80FD;&#x4E3A;nil&#xFF1B;<code>customerVariables</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setPeopleVariable API原型
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// setPeopleVariable API调用示例一
[Growing setPeopleVariableWithKey:@"gender" andStringValue:@"male"];
```

```objectivec
// setPeopleVariable API调用示例二
[Growing setPeopleVariable:@{@"gender":@"male", @"age":@"25"}];
```

### setVisitor

#### 2.4.0以上版本支持

当用户未登录时，定义用户属性变量，也可用于A/B测试上传标签。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| variable | JSON Object | 是 | 访问用户信息 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">variable</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x80FD;&#x4E3A;<code>nil&#xFF1B;variable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setVisitor 访问用户变量 API原型
+ (void)setVisitor:(NSDictionary<NSString *, NSObject *> *)variable;
```

```text
// setVisitor API调用示例
[Growing setVisitor:@{@"gender":@"male", @"age":@"25"}];
```

### setUserId

当用户登录之后调用setUserId API，设置登录用户ID。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| userId | String | 是 | 用户的登录用户ID |

**参数限制条件：**

| 参数名称 | 限制条件 |
| :--- | :--- |
| userId | 英文数字组合的字符串，长度小于等于1000，且不能含有特殊字符，不允许传空、`""` 或者`nil`，如有清除操作，请调用 `clearUserId` 方法 |

```objectivec
// setUserId API原型
+ (void)setUserId:(NSString *)userId;
```

```objectivec
// setuserId API调用示例
[Growing setUserId:@"1234567890"];
```

{% hint style="warning" %}
如果您的应用是App，且每次用户升级App版本时无需重新登录的话，建议在用户每次升级App版本后初次访问时重新调用上述 setUserId 方法。
{% endhint %}

### clearUserId

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

```objectivec
// clearUserId API原型
+ (void)clearUserId;
```

```objectivec
// clearUserId API调用示例
[Growing clearUserId];
```

## 验证 SDK 是否正常工作

GrowingIO 会采集发送两种类型的事件，在不做特殊设置的前提下，您会在 log 或 Mobile Debugger 里看到以下列表里的事件数据。

#### 类型一：无埋点事件类型列表

| 事件名称 | 发送时机 | t 字段值 |
| :--- | :--- | :--- |
| page 事件 | 每当进入一个页面时会发送一个page事件 | page |
| vst 事件 | 每当产生一个新的访问时 | vst |
| activate  事件 | 当 App 首次激活打开时 | avtivate |
| reengage 事件 | 由 GrowingIO Deep Link 唤醒App时 | reengage |
| clck 事件 | 当用户对 App 上的可点击元素有点击行为时 | clck |
| imp 事件 | 当有元素展现时 | imp |
| chng 事件 | 当用户对 App 上的输入元素有改变的行为时 | chng |

#### 类型二：埋点事件类型列表

| 事件名称       | 发送时机 | t 字段值        | var 字段值 | 其它字段        |
| :--- | :--- | :--- | :--- | :--- |
| cstm 事件        | 调用 track 类型的接口时 | cstm           | 若您设置了事件级变量 | n 为事件标识符，num为事件数值 |
| pvar 事件 | 调用 setPageVariable 接口时 | pvar | 若您设置了页面级变量     | 无 |
| evar 事件 | 调用 setEvar 类型接口时 | evar | 若您设置了转化变量 | 无 |
| ppl 事件 | 调用 setPeopleVariable 类型的接口时 | ppl | 若您设置了用户变量 | 无 |
| vstr 事件 | 调用 setVisitor 类型接口时 | vstr | 若您设置了访问用户变量       | 无 |

### 1. Mobile Debugger

Mobile Debugger 详细介绍，请参考文档 [GrowingIO Debugger](../growingio-debugger/) ，以下简要操作步骤主要用于验证SDK 是否正常工作。

1. 登录官网 ---&gt; 点击项目选择框 ---&gt; 点击 "项目管理" ---&gt; 选择 "Mobile Debugger"
2. 按照官网说明，启动 Mobile Debugger
3. 验证事件是否发送成功，数据是否正确

### 2. 无埋点事件和自定义事件验证

1. 启动 "Mobile Debugger" 
2. 在 "Mobile Debugger" 界面内，通过 t 字段值快速定位您要验证的事件
3. 查看事件其它字段的值是否符合预期

### 3. 圈选和热图功能验证

1. 启动圈选功能
2. 点击小红点，打开设置菜单
3. 打开 "显示热图" 开关后，如果相应页面有热图数据，则会显示热图
4. 拖动小红点，查看元素是否能被圈选，同时查看圈选相关数据是否被准确设置



