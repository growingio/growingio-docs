# iOS SDK

*  [集成 SDK](ios-sdk.md#ji-cheng-sdk)
  * [选择集成方式](ios-sdk.md#1-xuan-ze-ji-cheng-fang-shi)
  * [设置 URL Scheme](ios-sdk.md#2-she-zhi-url-scheme)
  * [初始化](ios-sdk.md#3-chu-shi-hua)
* [重要配置](ios-sdk.md#zhong-yao-pei-zhi)
  * [采集广告 Banner  数据](ios-sdk.md#cai-ji-guang-gao-banner-shu-ju)
  * [采集输入框数据](ios-sdk.md#cai-ji-shu-ru-kuang-shu-ju)
  * [Facebook 广告 SDK](ios-sdk.md#facebook-guang-gao-sdk)
  * [采集 WebView 页面数据](ios-sdk.md#cai-ji-webview-ye-mian-shu-ju)
  * [采集 GPS 数据](ios-sdk.md#cai-ji-gps-shu-ju)
  * [采集 HashTag数据](ios-sdk.md#cai-jih5-ye-mian-shu-ju)
  * [GDPR 数据采集开关](ios-sdk.md#gdpr-shu-ju-cai-ji-kai-guan)
  * [DeepLink回调参数获取](ios-sdk.md#deeplink-hui-tiao-can-shu-huo-qu)
  * [Universal Link 链接](ios-sdk.md#universal-link-lian-jie)
  * [设置界面元素 ID](ios-sdk.md#she-zhi-jie-mian-yuan-su-id)
  * [在 App Store 提交应用](ios-sdk.md#zai-app-store-ti-jiao-ying-yong)
* [自定义事件和变量 API 说明](ios-sdk.md#zi-ding-yi-shi-jian-he-bian-liang-api)
  * [track](ios-sdk.md#track)
  * [setPageVariable](ios-sdk.md#setpagevariable)
  * [setEvar](ios-sdk.md#setevar)
  * [setPeopleVariable](ios-sdk.md#setpeoplevariable)
  * [setVisitor](ios-sdk.md#setvisitor)
  * [setUserId](ios-sdk.md#setuserid)
  * [clearUserId](ios-sdk.md#clearuserid)
* [验证 SDK 是否正常工作](ios-sdk.md#yan-zheng-sdk-shi-fou-zheng-chang-gong-zuo)
  * [Mobile Debugger](ios-sdk.md#1-mobile-debugger)
  * \*\*\*\*[无埋点事件和自定义事件验证](ios-sdk.md#2-wu-mai-dian-shi-jian-he-zi-ding-yi-shi-jian-yan-zheng)
  * [圈选和热图功能验证](ios-sdk.md#3-quan-xuan-he-re-tu-gong-neng-yan-zheng)
* [1.x 旧版本 SDK 升级指导](ios-sdk.md#sdk-1x-jiu-ban-ben-sheng-ji-zhi-dao)
  * [重新集成 SDK](ios-sdk.md#1-zhong-xin-ji-cheng-sdk)
  * [迁移用户属性字段（CS字段）](ios-sdk.md#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [迁移页面属性字段（PS字段）](ios-sdk.md#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [迁移自定义事件（埋点事件）](ios-sdk.md#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [数据校验](ios-sdk.md#5-shu-ju-xiao-yan)
* [集成 1.x 旧版本 SDK](ios-sdk.md#ji-cheng-sdk-1x-ban-ben)

## 集成 SDK 

在您的 iOS  项目中集成 GrowingIO SDK，使用 GrowingIO 提供的多种工具来分析用户行为。

### 1. 选择集成方式

#### （1）使用 CocoaPods 快速集成

* 添加`pod 'GrowingIO', '~>2.4.3'`到 Podfile 中
* 执行`pod update`，不要用`--no-repo-update`选项
* 直接进行第 2 步 [“设置 URL Scheme”](ios-sdk.md#2-she-zhi-url-scheme)

#### （2）手动集成 SDK 

* [下载 2.4.3 版 iOS SDK](https://assets.growingio.com/sdk/GrowingIO-iOS-SDK-2.4.3.zip)
* 解压 iOS SDK 压缩文件
* 将 Growing.h 和 libGrowing.a 添加到 iOS 工程

![](https://www.growingio.com/vassets/javascripts/img-3-VLO4K.png)

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

{% hint style="warning" %}
#### 提醒：添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries
{% endhint %}

* 添加编译参数

![](https://www.growingio.com/vassets/javascripts/img-3e3i3Wq.png)

### 2. 设置 URL Scheme

####    2.1 获取 URL Scheme

* 添加新产品：登录官网 -&gt; 点击项目选择框  -&gt; 点击“设置”icon -&gt; 点击“新建应用”  -&gt; 选择添加 iOS 应用 -&gt; 填写“应用名称”，点击下一步 -&gt; 在第二段中标黄字体。
* 现有产品：登录官网  -&gt;   点击“设置”icon  -&gt;  点击“应用管理”  -&gt;  找到对应产品的 URL Scheme

![&#x5E94;&#x7528;&#x7BA1;&#x7406;&#x5165;&#x53E3;](../.gitbook/assets/image%20%2861%29.png)

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

![&#x9879;&#x76EE;&#x7BA1;&#x7406;&#x9875;&#x9762;&#x5165;&#x53E3;](../.gitbook/assets/image%20%289%29.png)

![&#x9879;&#x76EE;ID&#x67E5;&#x770B;](../.gitbook/assets/image%20%2841%29.png)

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

### 采集广告 Banner 数据

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

此外，当您想采集一些可能没有文字的控件（比如UIImageView，UIView）时，也可以给属性growingAttributesValue 赋值作为文字，用来在圈选的时候区分不同的内容。

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

### DeepLink 回调参数获取 {#deeplink-hui-tiao-can-shu-huo-qu}

推广过程中，您需要已经下载您`APP`的用户扫描二维码直接跳转您的`APP`中指定的活动页面，并根据实际情况显示不同信息，`SDK 2.3.2`以上版本支持。

例如：扫码领取优惠券，已经安装`APP`的用户扫码后将直接进入领取优惠券页面，并且这个页面上还能够显示此用户头像等信息，这个时候您可以使用我们的`DeepLink`功能，通过此接口获得您的自定义参数信息。

```objectivec
// params 为解析正确时反回调的参数, error 为解析错误时返回的参数.
// handler 默认为空, 客户需要手动设置.
+ (void)registerDeeplinkHandler:(void(^)(NSDictionary *params, NSError *error))handler;
```

```objectivec
//DeepLink 调用实例
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

###  **Universal Link 链接**

如果使用 Universal Link 唤醒APP， 请添加以下代码，注册 Universal Link 参数解析回调方法与DeepLink一致。

```objectivec
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray * _Nullable))restorationHandler{
    [Growing handleUrl:userActivity.webpageURL];
	return YES;
}
```

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

## 自定义事件和变量 API 

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](/docs/~/drafts/-LI499co1_eo3lOYex8t/primary/data-defination/events-metrics/manual-metrics)和[变量](/docs/~/drafts/-LI499co1_eo3lOYex8t/primary/data-defination/dimensions/manual-dimensions)，下面介绍自定义事件和变量 API 使用方法。

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

| 参数名称 | 限制条件 |
| :--- | :--- |
| eventId | 英文数字组合的字符串，不能为 nil 或者""，长度小于等于50，且不能含有特殊字符 |
| number | 正整数或浮点数 |
| eventLevelVariable | 不能为 nil |

```objectivec
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

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 页面级别的信息 |

**参数限制条件：**

| 参数名称 | 限制条件 |
| :--- | :--- |
| key | 不能为 nil 或者""，长度小于等于50 |
| value | 不能为 nil 或者""，若为字符串则长度应大于 0 小于等于 1000 |
| pageLevelVariable | 不能为 nil |

```objectivec
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

| 参数名称 | 限制条件 |
| :--- | :--- |
| key | 不能为 nil 或者""，长度小于等于50 |
| Value | 变量不为nil或者""，若为字符串则长度应大于 0 小于等于 1000 |
| conversionLevelVariable | 不能为nil |

```objectivec
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

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 用户变量用于用户信息相关的分析 |

**参数限制条件：**

| 参数名称 | 限制条件 |
| :--- | :--- |
| key | 不能为nil或""，长度小于等于50 |
| value | 变量不为nil或者""，若为字符串则长度应大于 0 小于等于 1000 |
| customerVariables | 不能为nil |

```objectivec
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

| 参数名称 | 限制条件 |
| :--- | :--- |
| variable | 不能为`nil` |

```objectivec
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

Mobile Debugger 详细介绍，请参考文档[《GrowingIO Debugger》](https://growingio.gitbook.io/docs/~/edit/drafts/-LI499co1_eo3lOYex8t/sdk-integration/growingio-debugger#growingio-mobile-debugger)，以下简要操作步骤主要用于验证SDK 是否正常工作。

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

## SDK 1.x 旧版本升级指导

{% hint style="danger" %}
#### 升级到2.x SDK前，请务必联系客服协助您完成后台配置的升级！
{% endhint %}

本文旨在帮助您从 1.x 版本无缝升级至 2.x 版本，由于两个版本的接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

您需要完成以下几个步骤： 

### 1. 重新集成 SDK

请您参考以下开发文档，完成SDK初始化代码的添加。

* [iOS SDK 开发文档](ios-sdk.md#ji-cheng-sdk)

{% hint style="info" %}
建议您在开发中，开启 Growing 调试日志来检验 SDK 的数据是否正常上传
{% endhint %}

### **2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“[用户变量]()”。两者的区别主要在于：用户变量支持自定义的归因方式。

#### **2.1 上传接口：**

2.x 版本中的上传用户变量方法有较大改动，不再将 `setCSn`  这个字段作为参数，方法中只需写入用户变量的 key - value 对。

* 1.x 版本方法格式：

```objectivec
[Growing setCS1Value:@"100324" forKey:@"user_id"];
[Growing setCS2Value:@"943123" forKey:@"company_id"];
[Growing setCS3Value:@"张溪梦" forKey:@"user_name"];
```

* 2.x 版本方法格式：

#### 对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```objectivec
// 设置登录用户ID API
+ (void)setUserId:(NSString *)userId;

// 清除登录用户ID API
+ (void)clearUserId;
```

#### 对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```objectivec
[Growing setAppVariable:@{@"key1":@"value1", @"key2":@2}];
[Growing setAppVariableWithKey:@"key1" andStringValue:@"value1"];
[Growing setAppVariableWithKey:@"key2" andNumberValue:@2];
```

#### 对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```objectivec
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable; // 多个变量，可组合为一个对象传入
```

#### **2.2 GrowingIO 后台配置**

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档]()。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量”]()。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

#### **3.1 上传接口**

* 1.x 版本方法格式：

```objectivec
@property (nonatomic, copy) NSString* growingAttributesPageGroup;
@property (nonatomic, copy) NSString* growingAttributesPS1;
@property (nonatomic, copy) NSString* growingAttributesPS2;
@property (nonatomic, copy) NSString* growingAttributesPS3;
```

* 2.x 版本方法格式：

```objectivec
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;

+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;

+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
```

#### 3**.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档]()

### 4. 迁移自定义事件（埋点事件）

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

#### **4.1 上传接口**

* 1.x 版本方法格式：

```objectivec
@interface Growing: NSObject
+ (void)track: (NSString *) event properties: (nullable NSDictionary *) properties;
@end
```

* 2.x 版本方法格式：

```objectivec
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

#### **4.2 GrowingIO 后台配置**

您可以在 GrowingIO 官网的 “数据管理” ---&gt; “事件和变量” 页面找到管理事件，事件级变量，页面级变量，应用级变量，转化变量和用户变量的配置。

### 5. 数据校验

完成上述工作后，我们需要对数据是否成功上传进行校验，GrowingIO 提供 SDK debug 模式和 [Mobile Debugger 工具](https://growingio.gitbook.io/docs/~/edit/drafts/-LI499co1_eo3lOYex8t/sdk-integration/growingio-debugger#growingio-mobile-debugger)，来帮助您完成数据的校验。

#### 5.1  SDK debug 模式校验

在下面方法中添加 `[Growing setEnableLog:YES]`开启调试日志。

```objectivec
#import "Growing.h"

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      ...
      // 启动GrowingIO
      [Growing startWithAccountId:@"xxxxxxxxxxxxxxxx"]; //替换为您的ID
      // 其他配置
      // 开启Growing调试日志，可在log中校验事件数据
      [Growing setEnableLog:YES];
  }
```

开启 debug 模式后，您需要在app上触发打点事件，在 log 里搜索上述关键字就能找到对应自定义事件和变量上传的数据。

**5.2 GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” ---&gt; “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您首先需要确保您的自定义事件或变量确实有被触发。

#### 5.3 使用 Mobile Debugger 验证

Mobile Debugger 使用方式见[“验证SDK是否正常工作”](ios-sdk.md#yan-zheng-sdk-shi-fou-zheng-chang-gong-zuo)。

## 集成 SDK 1.x 版本

{% hint style="danger" %}
1.x 版本已停止更新，如果您集成的是 1.x 版本，请联系客服帮助您升级到 2.x 版本，新用户请勿集成 1.x 版本！
{% endhint %}

### 1. 选择集成方式

#### （1）使用 CocoaPods 快速集成

* 添加`pod 'GrowingIO', '~>1.4.2'`到 Podfile 中
* 执行`pod update`，不要用`--no-repo-update`选项
* 直接进行第 2 步 [“设置 URL Scheme”](ios-sdk.md#2-she-zhi-url-scheme)

#### （2）手动集成 SDK 

* 点击[1.4.2版](http://assets.growingio.com/sdk/GrowingIO-iOS-SDK-1.4.2.zip)下载 iOS SDK
* 解压 iOS SDK 压缩文件
* 将 Growing.h 和 libGrowing.a 添加到 iOS 工程

![](https://www.growingio.com/vassets/javascripts/img-3-VLO4K.png)

#### **提醒:** 记得勾选 "Copy items if needed" {#ti-xing-ji-de-gou-xuan-copy-items-if-needed}

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

{% hint style="warning" %}
#### 提醒：添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries {#ti-xing-tian-jia-xiang-mu-yi-lai-ku-de-wei-zhi-zai-xiang-mu-she-zhi-target-xuan-xiang-ka-general-linked-frameworks-and-libraries}
{% endhint %}

* 添加编译参数

![](https://www.growingio.com/vassets/javascripts/img-3e3i3Wq.png)

### 2. 设置 URL Scheme

####    2.1 获取 URL Scheme

* 添加新产品：登录官网 -&gt; 点击项目选择框  -&gt; 点击“项目管理”  -&gt; 点击“应用管理” -&gt; 点击“新建应用”  -&gt; 选择添加 iOS 应用 -&gt; 填写“应用名称”，点击下一步 -&gt; 在第二段中标黄字体。
* 现有产品：登录官网  -&gt; 点击项目选择框  -&gt;  点击“项目管理”  -&gt;  点击“应用管理”  -&gt;  找到对应产品的 URL Scheme

![&#x9879;&#x76EE;&#x7BA1;&#x7406;](https://docs.growingio.com/.gitbook/assets/image.png)

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

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    // 启动GrowingIO
    [Growing startWithAccountId:@"xxxxxxxxxxxxxxxx"]; //替换为您的ID
    // 其他配置
    // 开启Growing调试日志 可以开启日志
    // [Growing setEnableLog:YES];
}
```

{% hint style="warning" %}
#### 提醒：请确保将代码添加在上述位置，添加到其他方法或异步block中可能导致数据不准确！
{% endhint %}

### 4. 重要配置 {#7-重要配置项}

{% hint style="danger" %}
#### 下述各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要！ {#下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要}
{% endhint %}

#### 4.1 设置界面元素ID

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

#### 4.2 采集广告 Banner 数据 {#采集广告banner数据}

如果您的 app 上方有横向滚动的 Banner 广告，若要收集 Banner 相关数据，请在响应点击的控件上添加如下代码：

```objectivec
UIView *view;
…
view.growingAttributesValue = 广告的唯一ID;
```

其中 view 是您的广告元素，请确保两点：

* 对不同广告图，广告的唯一 ID 也不相同
* 响应点击的控件，与设置 ID 的控件是同一个

【例如】当您的横向滚动广告共有3张广告图时，您可以在3个响应点击的View上分别设置不同的广告唯一ID，实现方式：

```objectivec
view1.growingAttributesValue = @"ad1";
view2.growingAttributesValue = @"ad2";
view3.growingAttributesValue = @"ad3";
```

此外，当您想采集一些可能没有文字的控件（比如UIImageView，UIView）时，也可以给属性growingAttributesValue 赋值作为文字，用来在圈选的时候区分不同的内容。

#### 4.3 页面别名 {#页面别名}

对于iOS应用，页面指的是`UIViewController`。

有些时候，对于完成某个功能的页面，统计时可能需要进一步细分。 比如，对于展示商品列表的页面，需要区分衣物类商品，以及食品类商品的两种列表的访问量。

为处理这种场景，我们提供了取别名的方法来区分这两种情况下的页面，接口如下：

```objectivec
@interface UIViewController(GrowingAttributes)
@property (nonatomic, copy)   NSString* growingAttributesPageName;
@end
```

**【例子】**

1. 某个应用的商品列表页是用`ListViewController`实现的，所以默认的页面名称都是`ListViewController`。
2. 如果想区分衣物类商品列表和食品类商品列表，分别看它们的浏览量，可以这样做：

   ```objectivec
   //ListViewController类的实现文件
   -(void)viewWillAppear
   {
    if (展示衣物类商品)
    {
        self.growingAttributesPageName = @"clothe_item_list";
    }
    else if (展示食品类商品)
    {
        self.growingAttributesPageName = @"food_item_list";
    }
   }
   ```

{% hint style="warning" %}
注意：

1. 必须在该 UIViewController 的 viewWillAppear 或者更早时机的函数中完成该属性的赋值操作。
2. 页面别名只能设置为字母、数字和下划线的组合。
3. 为查看数据方便，请尽量对iOS和安卓的同功能页面取不同的名称。
{% endhint %}

#### 4.4  采集输入框数据 {#采集输入框数据}

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

#### 4.5  Facebook 广告 SDK {#facebook广告sdk}

如果您使用了 Facebook 广告 SDK，请务必在 main 函数第一行调用以下代码来避免冲突，否则可能造成无法创建项目或者统计准确性问题。注意：APP启动后，将不允许修改采集模式。

```objectivec
[Growing setAspectMode:GrowingAspectModeDynamicSwizzling];
```

#### 4.6  采集 WebView 页面数据 {#采集h5页面数据}

SDK 会自动采集H5页面的数据，不需要特殊配置。

#### 4.7  采集GPS数据 {#采集gps数据}

如果您的应用有相应权限，SDK 将自动采集您的GPS数据。

#### 4.8  启用Hashtag识别 {#77-启用hashtag识别}

您可以在项目中添加以下方法以启用 Hashtag 识别：

```objectivec
// 设置为 YES, 将启用 HashTag
+ (void)enableHybridHashTag:(BOOL)enable;
```

### 5. 其他配置项 {#8-其他配置项}

#### 5.1 自定义维度 {#自定义维度}

GrowingIO的数据分析工具提供了例如“应用版本”，“渠道”，“城市”，“设备型号”等等这些通用维度。但在使用过程中，有时无法满足用户对特定数据维度的分析要求。

为了能够让数据分析变得更加的灵活，我们提供了自定义维度的接口，使用者可以对各种对象设置属性。

**这些属性在作图时，将表现为维度。**

**5.2 用户属性**

用户属性只能用来表示登录用户本身的属性，至少包括用户ID。根据需求，可以用来指定用户的各种属性：

 1. 自然属性，比如性别、出生年月等。

 2. 账户属性，比如等级、类型标签等。

 3. 行为特征，比如是否有过购买记录之类。

用户属性被称为CS字段，最多支持十个，从CS1到CS10。

**【例子】我们举例说明它的用法**

总计上传5个用户属性，分别是

CS1: user\_id:100324

CS2: company\_id:943123

CS3: user\_name:张溪梦

CS4: company\_name:GrowingIO

CS5: sales\_name:销售员小王

用户可以填入自定义的数据，该数据不会持久化，在数据变化的时候填入即可

```objectivec
-(void)someMethod
{
    …
    [Growing setCS1Value:@"100324" forKey:@"user_id"];
    [Growing setCS2Value:@"943123" forKey:@"company_id"];
    [Growing setCS3Value:@"张溪梦" forKey:@"user_name"];
    [Growing setCS4Value:@"GrowingIO" forKey:@"company_name"];
    [Growing setCS5Value:@"销售员小王" forKey:@"sales_name"];
    …
}
```

**CS字段设置条件和限制**

1. CS 字段不能是和用户没有直接关系的属性，比如不能是订单 ID，商品 ID 等。
2. CS1 字段：在 GrowingIO 系统中用于识别注册用户的身份，因此 CS1 的 value 必须填写用户的唯一身份标示 ID。
3. CS2 字段：在 GrowingIO 系统中用于识别 SaaS 客户的租户，因此所有的 SaaS 用户必须填写租户的唯一身份标示 ID，非 SaaS 用户不做限定。
4. 对于未登录用户，不要设置任何CS字段。
5. 如果没有用到所有的CS字段，剩下的可以不设置。
6. 同一个CS字段，必须保持在各个平台意义相同。

**5.3  对于CS1字段设置时机的说明**

* **基本原则**
  * 当App使用者的登录状态改变时设置CS1字段的值
  * 设置后，在下一次任意`viewDidAppear`被调用时，新的CS1字段的值才会被发送
  * 在CS1字段的值被发送前，重复设值会导致新的值取代旧的，旧值会被丢弃
* **用户手动登录**
  * 如果有多个登录入口，在每一个入口登录成功后，都需要调用`[Growing setCS1Value:forKey:]`来设置用户的唯一标识
  * 如果有第三方登录，成功登录后需要调用`[Growing setCS1Value:forKey:]`方法
* **自动登录**
  * App启动时，自动登录或者用户默认是登录状态，也需要调用`[Growing setCS1Value:forKey:]`方法
* **注册**

  * 有的App注册成功后，默认登录，这种情况下也需要调用`[Growing setCS1Value:forKey:]`方法

* **退出**
  * 退出登录后，请勿继续上传CS字段，即使是空值也请勿上传。

{% hint style="info" %}
1. **其他CS字段遵循相似的设置方法**
2. **在上传成功两小时后，您需要在「项目管理-项目配置-CS 配置中」进行字段配置和激活，配置成功后便可开始使用 CS 字段进行分析。**
{% endhint %}

#### 5.4 设置元素对象 {#设置元素对象}

如果元素自身的内容并不能代表具体的意义，可以使用元素对象来标识。

例如“加入购物车”按钮，可以设置成加入购物车的具体商品名称或ID。

```objectivec
UIView * view;
...
view.growingAttributesInfo = "The New iPad";
```

#### 5.5 忽略某元素 {#忽略某元素}

```objectivec
UIView * view;
...
view.growingAttributesDonotTrack = YES;
```

其中，view是您需要忽略的元素。

#### 5.6 自定义数据接收服务器 {#自定义数据接收服务器}

GrowingIO SDK 支持自定义行为数据接收服务器，用户可以选择数据先采集到自己的服务器做一些处理（比如审计）然后转发到 GrowingIO 服务器。详情参见 [CustomTrackerHost](https://github.com/growingio/help_site/tree/f4b4103b288205f6a9b13e0c4692f4d65a2ab386/Features/CustomTrackerHost.html)

#### 5.7 在 App Store 提交应用

集成了 GrowingIO SDK 以后，默认会启用 IDFA，所以在向 App Store 提交应用时，需要：

* 对于问题 **Does this app use the Advertising Identifier \(IDFA\)**，选择 YES。
* 对于选项**Attribute this app installation to a previously served advertisement**，打勾。
* 对于选项**Attribute an action taken within this app to a previously served advertisement**，打勾。

{% hint style="info" %}
#### 为**什么 GrowingIO 使用 IDFA?**

GrowingIO 使用 IDFA 来做来源管理激活设备的精确匹配，让你更好的衡量广告效果。如果你不希望跟踪这个信息，可以选择不引入 AdSupport.framework 或者在用 Cocoapods 安装时使用 ‘GrowingIO/without-IDFA' subspec.
{% endhint %}

至此，您的SDK安装就成功了。您登录 GrowingIO 进入产品安装页面执行“数据检测”几分钟后就可以看到数据了。

## 常见问题

#### 1. GrowingIO SDK 无埋点采集原理是什么？

    答：实现原理：[Method Swizzling](http://nshipster.cn/method-swizzling/) \( method swizzling 作用为替换或者修改系统方法\)，可理解为以下 5 个步骤。

> 1. 在系统方法 M1 中插入 GIO 代码 GIO\_B1
> 2. 系统方法 M1 被执行，此时也会执行 GIO\_B1
> 3. 在 GIO\_B1 内遍历 View Tree / DOM Tree 以及 UIViewController 的层次关系以获取必要的数据
> 4. 生成待发送事件并缓存
> 5. 发送事件到服务器，完成采集

![](https://growingio.atlassian.net/wiki/download/thumbnails/180519296/hook.png?version=1&modificationDate=1524817973586&cacheVersion=1&api=v2&width=265&height=250)

#### 2. GrowingIO SDK 支持哪些 iOS 系统？

    答：目前支持 iOS 7 ～ iOS 11。

#### 3. 如果动态添加 UIView、删除 UIView 或者 修改 UIView 在父视图中的位置，会有什么影响？

    答：SDK 依赖 subviews 里面的元素次序。如果有动态的需求，建议在 viewDidLoad 里加载所有可能的 UIView 节点，添加或删除可以通过设置 hidden 属性来实现，把不需要显示的设置 hidden 属性为 YES，把需要显示的设置 hidden 属性为 NO。在 UIView 已经显示之后，不要调用 -bringSubviewToFront:方法， -sendSubviewToBack:方法或 -insertSubview: 方法。

#### 4. 子线程操作 UI 引起的问题如何处理？

    答：GIO SDK 会在主线程中遍历找寻某个subView，如果此时在子线程中删除了该subView，就会造成错乱甚至 crash。此外，Apple 不建议用户在子线程更新 UI。建议客户在 Xcode 中打开 "Main Thread Checker" 检测线程使用是否合理。参考：[Main Thread Checker](https://developer.apple.com/documentation/code_diagnostics/main_thread_checker?language=objc)

#### 5. 圈选时，来自不同 VC 的元素显示的 VC 名称相同？

    答：这种情况，一般是因为父 VC 添加子 VC 方式不正确引起的，请客户排查子VC的生命周期是否完整。

#### 6. 是否允许设置 view 的 growingAttributesValue 为单个的数字或者字母？

    答：不允许这种设置。出于安全考虑，金融类 App 会自定义键盘（默认键盘容易被黑），如果 SDK 允许采集 V 值为单个数字或字母的点击事件，则有可能会记录用户输入的账号或密码。由于上述原因， SDK 不会发送V 值为单个字母或数字的点击事件，如果用户违反约定，则会导致某个元素只有浏览量而没有点击量。

#### 7. 打点为什么也要在主线程调用？

    答：打点数据涉及到 UI 元素，凡是涉及 UI 的我们都建议在主线程调用。

#### 8. 是否支持在开启热图的时候圈选？

    答：不支持，开启热图可能会导致被圈选元素的 x 值发生变化。

#### 9. 是否支持用 UITouch 实现的点击事件？

    答：不支持，建议使用 UITapGestureRecognizer

#### 10. GIO SDK 是否支持 Swift 项目？

    答：支持

#### 11. App 做了 Button 防重复点击，集成 SDK 后发现按钮无法点击？

    答： 请使用 GrowingIO 提供的防重复点击解决方式：[DJRepeatClickFilter](https://github.com/sishen/DJRepeatClickFilter/blob/master/TestClickQuickly/UIView%2BDJRepeatClickFilter.m)。

#### 12. 如果项目中使用了Firebase SDK，需要注意什么？

    答：如果您的 iOS 项目中集成了 Firebase SDK，请确保使用的 Firebase SDK 版本在 4.8.1 及以上，否则会造成数据采集不到的情况。



