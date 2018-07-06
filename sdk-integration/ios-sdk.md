# iOS SDK

* [集成 SDK 和 API 导入](ios-sdk.md#ji-cheng-sdk-he-api-dao-ru)
  * [1. SDK 代码安装](ios-sdk.md#1-sdk-dai-ma-an-zhuang)
  * [2. iOS SDK API](ios-sdk.md#ios-sdk-api)
  * [3. 自定义数据上传&配置指导](ios-sdk.md#3-zi-ding-yi-shu-ju-shang-chuan-pei-zhi-zhi-dao)
* [1.x 版本 SDK 升级指导](ios-sdk.md#sdk-1x-版本升级指导)
  * [1. 重新集成 SDK](ios-sdk.md#1-zhong-xin-ji-cheng-sdk)
  * [2. 迁移用户属性字段（CS字段）](ios-sdk.md#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [3. 迁移页面属性字段（PS字段）](ios-sdk.md#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [4. 迁移自定义事件（埋点事件）](ios-sdk.md#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [5. 数据校验](ios-sdk.md#5-shu-ju-xiao-yan)
* [GrowingIO Mobile Debugger](ios-sdk.md#growingio-mobile-debugger)
* [如何集成 1.  x 版本 SDK](ios-sdk.md#ru-he-ji-cheng-1x-ban-ben-sdk)

## 集成 SDK 和 API 导入

### 1. SDK 代码安装

如果您的 iOS 项目中集成了 Firebase SDK，请确保使用的 Firebase SDK 版本在 4.8.1 及以上，并且集成 GrowingIO SDK 2.1.1 及以上版本，否则会造成数据采集不到的情况。

#### 1. 选择 SDK 安装方式

请确保您的 XCode 版本为 7.3 或者其后的版本。

GrowingIO 支持两种 iOS SDK 安装方式：

一. 使用 CocoaPods 管理依赖

1. 添加`pod 'GrowingIO', '~>2.3.3'`到 Podfile 中
2. 执行`pod update`，不要用`--no-repo-update`选项
3. 直接进入安装文档的第四步

二. 手动安装依赖

1. 下载 [2.3.3](http://assets.growingio.com/sdk/GrowingIO-iOS-SDK-2.3.3.zip) 版 iOS SDK
2. 进行安装文档的第二步

#### 2. 导入 SDK

按照以下步骤将SDK导入到您的项目中：

1. 解压 iOS SDK 压缩文件
2. 将 Growing.h 和 libGrowing.a 添加到 iOS 工程

![](https://www.growingio.com/vassets/javascripts/img-3-VLO4K.png)

**提醒:**

* 记得勾选 "Copy items if needed"

#### 3. 添加依赖

在工程项目中添加以下库文件

| 库名称 | 说明 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Foundation.framework | 基础依赖库 |
| Security.framework | 用于APP连接圈选页面SSL连接 |
| CoreTelephony.framework | 用于读取运营商名称 |
| SystemConfiguration.framework | 用于判断网络状态 |
| AdSupport.framework | 用于来源管理激活匹配 |
| libicucore.tbd | 用于APP连接圈选页面解析 |
| libsqlite3.tbd | 存储日志 |
| CoreLocation.framework | 用于读取地理位置信息（如果您的app有权限） |

**添加完成以后，库的引用如下:**

**提醒：**

* 添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries

#### 4. 添加编译参数

![](https://www.growingio.com/vassets/javascripts/img-3e3i3Wq.png)

#### 5. 设置 URL Scheme

#### **（1）获取URL Scheme**

* 添加新产品：登录官网 -&gt; 点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt;选择添加 iOS 应用 -&gt; 填写“应用名称“，点击下一步 -&gt;在第二段中标黄字体。
* 现有产品：登录官网 -&gt; 点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的 URL Scheme

![&#x9879;&#x76EE;&#x7BA1;&#x7406;](https://docs.growingio.com/.gitbook/assets/image.png)

#### **（2）添加 URL Scheme（growing.xxxxxxxxxxxxxxxx）到项目中，以便唤醒您的程序进行圈选**

#### **（3）在 AppDelegate 中添加激活圈选的代码**

```text
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    if ([Growing handleUrl:url])
    {
        return YES;
    }
    ...
    return NO;
}
```

提醒：

* 如果您的 AppDelegate 中，实现了其中一个或者多个方法，请在已实现的函数中，调用`[Growing handleUrl:]`:

```text
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
```

* 如果以上所有函数都未实现，则请实现以下方法并调用`[Growing handleUrl:]`:

```text
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
```

* 实际情况可能很复杂，请在调试时确保函数`[Growing handleUrl:]`会被执行到

#### 6. 添加初始化函数

在 AppDelegate 中引入`#import "Growing.h"`并添加启动方法

```text
#import "Growing.h"

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      ...
      // 启动GrowingIO
      [Growing startWithAccountId:@"xxxxxxxxxxxxxxxx"]; //替换为您的ID
      // 其他配置
      // 开启Growing调试日志 可以开启日志
      // [Growing setEnableLog:YES];
  }
```

_**请确保将代码添加在上面描述的位置，添加到其他函数中或者异步 block 中可能导致数据不准确！**_

#### 7. 重要配置项

#### 7.1 设置界面元素ID

当您的应用界面改版时，可能会导致无法准确地统计已经圈选的元素。因此，对于应用中的主要流程涉及到的界面元素，建议您为它们设置固定的唯一ID，以保证数据的一致性。

具体要求：

* 主要流程指的是登录、注册、购买、发帖等操作步骤。
* 设置ID的对象是界面的重要按钮等元素，如“注册”、“结算”、“发布”按钮。

设置ID的方法如下：

（1）接口

```text
@interface UIView(GrowingAttributes)
@property (nonatomic, copy)NSString *growingAttributesUniqueTag;
@end
```

（2）代码写法：请加在viewWillAppear或者时机更早的函数里。

```text
-(void)viewWillAppear
{
    UIView *MyView;
    …
    MyView.growingAttributesUniqueTag = @"my_view”;
}
```

（3）ID只能设置为字母、数字和下划线的组合。

（4）对于已经集成过旧版SDK并圈选过的应用，对某个元素设置ID后再圈选它，指标数值会从零开始计算，类似初次集成SDK后发版的效果，但不影响之前圈选的其它指标数据。如果不希望出现这种情况，请不要使用这个方法。

#### 7.2 采集广告Banner数据

很多应用上方都有横向滚动的Banner广告。对于这样的广告，如果要收集数据，请在响应点击的控件上添加如下代码

```text
UIView *view;
…
view.growingAttributesValue = 广告的唯一ID;
```

其中view是您的广告元素，请确保两点：

* 对不同广告图，广告的唯一ID也不相同
* 响应点击的控件，与设置ID的控件是同一个

例如，当您的横向滚动广告共有3张广告图时，您可以在3个响应点击的View上分别设置不同的广告唯一ID，类似如下效果：

```text
view1.growingAttributesValue = @“ad1”;
view2.growingAttributesValue = @“ad2”;
view3.growingAttributesValue = @“ad3”;
```

此外，当您想采集一些可能没有文字的控件（比如UIImageView，UIView）时，也可以给属性growingAttributesValue赋值作为文字，用来在圈选的时候区分不同的内容。

#### 7.3 采集输入框数据 {#采集输入框数据}

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置

`UIView *view; // view可以是UITextField, UITextView, UISearchBar ...`

`view.growingAttributesDonotTrackValue = NO;`

其中，view代表要被采集的输入框。 当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。 请注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。

#### 7.4 Facebook广告SDK {#facebook广告sdk}

如果使用了Facebook广告SDK，请务必添加以下代码来避免冲突，否则可能造成无法创建项目或者统计准确性问题。

请在main函数第一行调用下方函数。APP启动后，将不允许修改采集模式。

`[Growing setAspectMode:GrowingAspectModeDynamicSwizzling]`

#### 7.5 采集H5页面数据 {#采集h5页面数据}

会自动采集H5页面的数据，不需要特殊配置。

#### 7.6 采集GPS数据 {#采集gps数据}

如果您的应用有相应权限，我们将自动采集您的GPS数据。

#### 7.7 启用Hashtag识别 {#77-启用hashtag识别}

添加以下方法以启用Hashtag识别：

```text
    // 设置为 YES, 将启用 HashTag
    + (void)enableHybridHashTag:(BOOL)enable;
```

#### 8. 在 App Store 提交你的应用

集成了 GrowingIO SDK 以后，默认会启用 IDFA，所以在向 App Store 提交应用时，需要：

* 对于问题 **Does this app use the Advertising Identifier \(IDFA\)**，选择 YES。
* 对于选项**Attribute this app installation to a previously served advertisement**，打勾。
* 对于选项**Attribute an action taken within this app to a previously served advertisement**，打勾。

> **为什么 GrowingIO 使用 IDFA?**
>
> GrowingIO 使用 IDFA 来做来源管理激活设备的精确匹配，让你更好的衡量广告效果。如果你不希望跟踪这个信息，可以选择不引入 AdSupport.framework 或者在用 Cocoapods 安装时使用 ‘GrowingIO/without-IDFA' subspec.

至此，您的SDK安装就成功了。登录 GrowingIO 进入产品安装页面执行“数据检测”，几分钟后就可以看到数据了。

**其他设置（如设置“登录用户ID”）请前往**[ **iOS SDK API**](ios-sdk.md#ios-sdk-api)**中查看。**

### 2. iOS SDK API {#ios-sdk-api}

#### API简介 {#api简介}

```text
// 发送事件 API
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

// 设置登录用户ID API
+ (void)setUserId:(NSString *)userId;

// 清除登录用户ID API
+ (void)clearUserId;
```

#### track {#track}

发送一个事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面声明事件以及事件级变量。

参数：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariable | JSON Object | 否 | 事件发生时所伴随的维度信息。 |

```text
// track API原型
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```text
// track API调用示例一
[Growing track:@"registerSuccess"];
```

```text
// track API调用示例二
[Growing track:@"registerSuccess" withVariable:@{@"gender":@"male", @"age":@"21"}];
```

```text
// track API调用示例三
[Growing track:@"loanAmount" withNumber:@800000 andVariable:@{@"loanType":@"houseMortgage", @"province":@"Zhejiang"}];
```

#### setPageVariable {#setpagevariable}

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

参数：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 页面级别的信息 |

```text
// setPageVariable API原型
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
```

```text
// setPageVariable API调用示例一
[Growing setPageVariableWithKey:@"author" andStringValue:@"Zhang San" toViewController:myViewController];
```

```text
// setPageVariable API调用示例二
[Growing setPageVariable:@{@"pageName":@"Home Page", @"author":@"Zhang San"} toViewController:myViewController];
```

#### setEvar {#setevar}

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

参数

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 转化变量用于高级归因分析 |

```text
// setEvar API原型
+ (void)setEvarWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setEvarWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setEvar:(NSDictionary<NSString *, NSObject *> *)variable;
```

```text
// setEvar API调用示例一
[Growing setEvarWithKey:@"campaignId" andStringValue:@"1234567890"];
```

```text
// setEvar API调用示例二
[Growing setEvar:@{@"campaignId":@"12345", @"campaignOwner":@"Li Si"}];
```

#### setPeopleVariable {#setpeoplevariable}

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

参数：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 用户变量用于用户信息相关的分析 |

```text
// setPeopleVariable API原型
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```text
// setPeopleVariable API调用示例一
[Growing setPeopleVariableWithKey:@"gender" andStringValue:@"male"];
```

```text
// setPeopleVariable API调用示例二
[Growing setPeopleVariable:@{@"gender":@"male", @"age":@"25"}];
```

#### setUserId {#setuserid}

当用户登录之后调用setUserId API，设置登录用户ID。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- |
| userId | String | 是 | 用户的登录用户ID |

```text
// setUserId API原型
+ (void)setUserId:(NSString *)userId;
```

```text
// setuserId API调用示例
[Growing setUserId:@"1234567890"];
```

注：如果您的应用是App，且每次用户升级App版本时无需重新登录的话，建议在用户每次升级App版本后初次访问时重新调用上述 setUserId 方法。

#### clearUserId {#clearuserid}

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

```text
// clearUserId API原型
+ (void)clearUserId;
```

```text
// clearUserId API调用示例
[Growing clearUserId];
```

### 3. 自定义数据上传&配置指导

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，具体请参考[相关文档](../data-model/event-variable/custom-event.md)。

## SDK 1.x 版本升级指导

本文旨在帮助您从 [1.x 版本](ios-sdk.md#ru-he-ji-cheng-1x-ban-ben-sdk)无缝升级至 2.x 版本，由于两个版本的诸多接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

您需要完成以下几个步骤： 

### 1. 重新集成 SDK

请您参考以下开发文档，完成SDK初始化代码的添加。

* [iOS SDK 开发文档](ios-sdk.md#sdk-dai-ma-an-zhuang)

Tips：建议您在开发中，使用 debug mode 校验 GrowingIO SDK 的数据是否正常上传。开启 debug mode 的方式请见上述文档中初始化方法部分。

### **2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“[用户变量](../data-model/event-variable/custom-variable.md#用户变量)”。两者的区别主要在于：用户变量支持自定义的归因方式。

#### **2.1 上传接口：**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

* 1.x 版本方法格式：

```text
[Growing setCS1Value:@"100324" forKey:@"user_id"];
[Growing setCS2Value:@"943123" forKey:@"company_id"];
[Growing setCS3Value:@"张溪梦" forKey:@"user_name"];
```

* 2.x 版本方法格式：

对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```text
// 设置登录用户ID API
+ (void)setUserId:(NSString *)userId;

// 清除登录用户ID API
+ (void)clearUserId;
```

对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```text
[Growing setAppVariable:@{@"key1":@"value1", @"key2":@2}];
[Growing setAppVariableWithKey:@"key1" andStringValue:@"value1"];
[Growing setAppVariableWithKey:@"key2" andNumberValue:@2];
```

对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```text
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable; // 多个变量，可组合为一个对象传入
```

#### **2.2 GrowingIO 后台配置**

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md#自定义数据-上传步骤)。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量”](../data-model/event-variable/custom-variable.md#页面级变量)。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

#### **3.1 上传接口**

* 1.x 版本方法格式：

```text
@property (nonatomic, copy) NSString* growingAttributesPageGroup;
@property (nonatomic, copy) NSString* growingAttributesPS1;
@property (nonatomic, copy) NSString* growingAttributesPS2;
@property (nonatomic, copy) NSString* growingAttributesPS3;
```

* 2.x 版本方法格式：

```text
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;

+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;

+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
```

#### 3**.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md#c-ye-mian-ji-bian-liang-pei-zhi)

### 4. 迁移自定义事件（埋点事件）

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

#### **4.1 上传接口**

* 1.x 版本方法格式：

```text
@interface Growing: NSObject
+ (void)track: (NSString *) event properties: (nullable NSDictionary *) properties;
@end
```

* 2.x 版本方法格式：

```text
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

#### **4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在\*\*“管理” - “自定义事件和变量” 页面中的 “自定义事件” Tab 页\*\*。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

### 5. 数据校验

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。 对移动端的开发者，GrowingIO 的 SDK 提供了 debug 模式，在 SDK 初始化代码中可以找到。如下图： ![](../../.gitbook/assets/13%20%282%29.jpeg) 

![](https://docs.growingio.com/.gitbook/assets/13%20%282%29.jpeg)

开启 debug 模式后，您需要在app上触发一下打点事件，在打出的log里搜索上述关键字就能找到对应自定义事件&变量上传的数据。

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保您的自定义事件或变量确实有被触发。

## GrowingIO Mobile Debugger

GrowingIO Debugger是GrowingIO推出的调试 SDK所发送数据的工具。在GrowingIO Debugger的帮助下，实施工程师可以看到在什么样的页面上，在什么时机向GrowingIO发送了什么样的服务器请求。

在数据驱动增长的实践中，数据准确性一直以来是一个必须重视的课题。数据分析行业中有一句谚语：Garbage In, Garbage Out，简称GIGO。这句谚语告诉我们：如果我们在实施阶段的方案有纰漏导致收集的数据就是错误的，那么基于错误的数据得出的结论就一定是有问题的，所以我们必须重视GrowingIO产品的实施工作，来确保实施方案的严谨正确，从而保证收集到的数据是尽最大可能准确的。

而在GrowingIO产品的实施过程中，GrowingIO Debugger能够帮助工程师、技术实施顾问清楚的看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了GrowingIO Web Debugger的帮助，实施工程师可以通过高质量的实施，给分析师带来高质量高准确性的数据，这将大大有利于在企业内部养成数据驱动决策的文化和习惯。使用数据，使用高质量的数据帮助企业获得真正的增长。

[点击查看 GrowingIO Mobile Debugger 的安装和使用](growingio-debugger.md#growingio-mobile-debugger)。

## 如何集成 1.x 版本 SDK

### 1. 选择 SDK 安装方式 {#1-选择-sdk-安装方式}

请确保您的 XCode 版本为 7.3 或者其后的版本。

GrowingIO 支持两种 iOS SDK 安装方式：

A. 使用 CocoaPods 管理依赖

1. 添加 `pod 'GrowingIO', '~> 1.4.2'` 到 Podfile 中；
2. 执行 `pod update`，**不要用**`--no-repo-update`**选项**；
3. 直接进入**安装文档的第五步**；

B. 手动安装依赖

1. 点击[1.4.2版](http://assets.growingio.com/sdk/GrowingIO-iOS-SDK-1.4.2.zip)下载 iOS SDK
2. 进行安装文档的第二步

### 2. 导入 SDK {#2-导入-sdk}

按照以下步骤将SDK导入到您的项目中。 1. 解压 iOS SDK 压缩文件； 2. 添加 Growing.h 和 libGrowing.a 添加到您的 iOS 工程中；

提醒:

* 记得勾选 "Copy items if needed"

### 3. 添加依赖 {#3-添加依赖}

在您的工程项目中添加依赖

| 库名称 | 说明 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Foundation.framework | 基础依赖库 |
| Security.framework | 用于APP连接圈选页面SSL连接 |
| CoreTelephony.framework | 用于读取运营商名称 |
| SystemConfiguration.framework | 用于判断网络状态 |
| AdSupport.framework | 用于来源管理激活匹配 |
| libicucore.tbd | 用于APP连接圈选页面解析 |
| libsqlite3.tbd | 存储日志 |
| CoreLocation.framework | 用于读取地理位置信息（如果您的app有权限） |

提醒：

* 添加项目依赖库的位置在 项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries

### 4. 添加编译参数 {#4-添加编译参数}

在您的工程项目中添加编译参数

1. 找到 Linking 设置
2. 在 Other Linker Flags 中添加 -ObjC 参数，请注意大小写

提醒：

* Linking 设置位于 项目设置 target -&gt; 选项卡 Build Settings，左上角选择 All。

### 5. 添加初始化函数 {#5-添加初始化函数}

在 AppDelegate 中引入`#import "Growing.h"`并添加启动方法

```text
- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      ...
       // 启动GrowingIO
      [Growing startWithAccountId:@"您的项目ID"];

      // 其他配置
      // 开启Growing调试日志 可以开启日志
      // [Growing setEnableLog:YES];
  }
```

请确保将代码添加在上面描述的位置，添加到其他函数中或者异步 block 中可能导致数据不准确。

### 6. 添加URL Scheme {#6-添加url-scheme}

把 URL Scheme 添加到您的项目，以便我们唤醒您的程序，进行圈选。

#### URL Scheme 获取方式 {#url-scheme-获取方式}

1. 添加新产品：登录官网 -&gt; 点击项目选择框 -&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt;选择添加 iOS 应用 -&gt; 填写“应用名称“，点击下一步 -&gt;在第二段中标黄字体。
2. 现有产品：登录官网 -&gt; 点击项目选择框-&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的 URL Scheme。

![&#x9879;&#x76EE;&#x7BA1;&#x7406;](https://docs.growingio.com/.gitbook/assets/image%20%288%29.png)

#### URL Scheme 的格式是 growing.xxxxxxxxxxxxxxxx。 {#url-scheme-的格式是-growingxxxxxxxxxxxxxxxx。}

1. 添加您的 URL Scheme（growing.xxxxxxxxxxxxxxxx）到项目中，URL Scheme 位于项目设置 target -&gt; 选项卡 Info -&gt; URL Types；
2. 在 AppDelegate 中调用函数 \[Growing handleUrl:\] 来接收 URL

```text
- (BOOL)application:(UIApplication *)appation openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    if ([Growing handleUrl:url])
    {
        return YES;
    }
    ...
    return NO;
}
```

提醒：

* 如果您的 AppDelegate 中，实现了其中一个或者多个方法，请在已实现的函数中，调用 `[Growing handleUrl:]`:

```text
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
```

* 如果以上所有函数都未实现，则请实现以下方法并调用 `[Growing handleUrl:]`:

```text
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
```

* 实际情况可能很复杂，请在调试时确保函数 `[Growing handleUrl:]` 会被执行到

#### 下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要 {#下方各个配置项将影响统计的准确性，请务必仔细阅读，判断是否需要}

### 7. 重要配置项 {#7-重要配置项}

#### 设置界面元素ID {#设置界面元素id}

当您的应用界面改版时，可能会导致无法准确地统计已经圈选的元素。

因此，对于应用中的主要流程涉及到的界面元素，建议您对它们设置固定的唯一ID，以保证数据的一致性。

具体要求

1. 主要流程指的是登录、注册、购买、发帖等操作步骤。
2. 设置ID的对象是界面的重要按钮等元素，如“注册”、“结算”、“发布”按钮。
3. 设置ID的方法如下：
4. 接口

   ```text
   @interface UIView(GrowingAttributes)
   @property (nonatomic, copy)   NSString* growingAttributesUniqueTag;
   @end
   ```

5. 代码写法：请加在viewWillAppear或者时机更早的函数里。

```text
-(void)viewWillAppear
{
    UIView * MyView;
    …
    MyView.growingAttributesUniqueTag = @"my_view”;
}
```

* ID只能设置为字母、数字和下划线的组合。
* 对于已经集成过旧版SDK并圈选过的应用，对某个元素设置ID后再圈选它，指标数值会从零开始计算，类似初次集成SDK后发版的效果，但不影响之前圈选的其它指标数据。如果不希望出现这种情况，请不要使用这个方法。

#### 采集广告Banner数据 {#采集广告banner数据}

很多应用上方都有横向滚动的Banner广告。

对于这样的广告，如果要收集数据，请在响应点击的控件上添加如下代码

```text
UIView *view;
…
view.growingAttributesValue = 广告的唯一ID;
```

其中view是您的广告元素，请确保两点：

1. 对不同广告图，广告的唯一ID也不相同。
2. 响应点击的控件，与设置ID的控件是同一个

例如，当您的横向滚动广告共有3张广告图时，您可以在3个响应点击的View上分别设置不同的广告唯一ID，类似如下效果：

```text
view1.growingAttributesValue = @“ad1”;
view2.growingAttributesValue = @“ad2”;
view3.growingAttributesValue = @“ad3”;
```

此外，当您想采集一些可能没有文字的控件（比如UIImageView，UIView）时，也可以给属性growingAttributesValue赋值作为文字，用来在圈选的时候区分不同的内容。

#### 页面别名 {#页面别名}

对于iOS应用，页面指的是`UIViewController`。

有些时候，对于完成某个功能的页面，统计时可能需要进一步细分。 比如，对于展示商品列表的页面，需要区分衣物类商品，以及食品类商品的两种列表的访问量。

为处理这种场景，我们提供了取别名的方法来区分这两种情况下的页面，接口如下：

```text
@interface UIViewController(GrowingAttributes)
@property (nonatomic, copy)   NSString* growingAttributesPageName;
@end
```

**我们举例来具体说明它的用法。**

1. 某个应用的商品列表页是用`ListViewController`实现的，所以默认的页面名称都是`ListViewController`。
2. 如果想区分衣物类商品列表和食品类商品列表，分别看它们的浏览量，可以这样做：

   ```text
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

请注意

1. 必须在该 UIViewController 的 viewWillAppear 或者更早时机的函数中完成该属性的赋值操作。
2. 页面别名只能设置为字母、数字和下划线的组合。
3. 为查看数据方便，请尽量对iOS和安卓的同功能页面取不同的名称。

#### 采集输入框数据 {#采集输入框数据}

如果您需要采集应用内某个输入框内的文字（例如搜索框），请调用如下接口进行设置

`UIView * view; // view可以是UITextField, UITextView, UISearchBar ... view.growingAttributesDonotTrackValue = NO;`

其中，view代表要被采集的输入框。 当这个输入框失去焦点（包括应用退到后台），且输入框内容跟获取焦点前相比发生变化时，输入框内文字会被发送回GrowingIO。 请注意：对于密码输入框，即便标记为需要采集，SDK也会忽略，不采集它的数据。

#### Facebook广告SDK {#facebook广告sdk}

如果使用了Facebook广告SDK，请务必添加以下代码来避免冲突，否则可能造成无法创建项目或者统计准确性问题。

请在main函数第一行调用下方函数。APP启动后，将不允许修改采集模式。

`[Growing setAspectMode:GrowingAspectModeDynamicSwizzling]`

#### 采集H5页面数据 {#采集h5页面数据}

会自动采集H5页面的数据，不需要特殊配置。

#### 采集GPS数据 {#采集gps数据}

如果您的应用有相应权限，我们将自动采集您的GPS数据。

#### 启用Hashtag识别 {#启用hashtag识别}

对于 1.3.1 及以上 SDK 版本，添加以下方法以启用Hashtag识别：

```text
    // 设置为 YES, 将启用 HashTag
    + (void)enableHybridHashTag:(BOOL)enable;
```

### 8. 其他配置项 {#8-其他配置项}

#### 自定义维度 {#自定义维度}

GrowingIO的数据分析工具提供了例如“应用版本”，“渠道”，“城市”，“设备型号”等等这些通用维度。但在使用过程中，有时无法满足用户对特定数据维度的分析要求。

为了能够让数据分析变得更加的灵活，我们提供了自定义维度的接口，使用者可以对各种对象设置属性。

**这些属性在作图时，将表现为维度。**

**用户属性**

用户属性只能用来表示登录用户本身的属性，至少包括用户ID。

根据需求，可以用来指定用户的各种属性 1. 自然属性，比如性别、出生年月等。 2. 账户属性，比如等级、类型标签等。 3. 行为特征，比如是否有过购买记录之类。

用户属性被称为CS字段，最多支持十个，从CS1到CS10。

**我们举例说明它的用法。**

总计上传5个用户属性，分别是

CS1: user\_id:100324

CS2: company\_id:943123

CS3: user\_name:张溪梦

CS4: company\_name:GrowingIO

CS5: sales\_name:销售员小王

用户可以填入自定义的数据，该数据不会持久化，在数据变化的时候填入即可

```text
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

**对于CS1字段设置时机的说明**

基本原则

1. 当App使用者的登录状态改变时设置CS1字段的值
2. 设置后，在下一次任意`viewDidAppear`被调用时，新的CS1字段的值才会被发送
3. 在CS1字段的值被发送前，重复设值会导致新的值取代旧的，旧值会被丢弃

用户手动登录 1. 如果有多个登录入口，在每一个入口登录成功后，都需要调用`[Growing setCS1Value:forKey:]`来设置用户的唯一标识。 2. 如果有第三方登录，成功登录后需要调用`[Growing setCS1Value:forKey:]`方法。

自动登录：App启动时，自动登录或者用户默认是登录状态，也需要调用`[Growing setCS1Value:forKey:]`方法

注册：有的App注册成功后，默认登录，这种情况下也需要调用`[Growing setCS1Value:forKey:]`方法。

退出：退出登录后，请勿继续上传CS字段，即使是空值也请勿上传。

**其他CS字段遵循相似的设置方法**

**在上传成功两小时后，您需要在「项目管理-项目配置-CS 配置中」进行字段配置和激活，配置成功后便可开始使用 CS 字段进行分析。配置过程请参考** [**属性数据\(CS\)上传配置文档**](../data-model/event-variable/custom-event.md#2-zai-shi-jian-he-bian-liang-zhong-wan-cheng-pei-zhi)**。**

#### 设置元素对象 {#设置元素对象}

如果元素自身的内容并不能代表具体的意义，可以使用元素对象来标识。

例如“加入购物车”按钮，可以设置成加入购物车的具体商品名称或ID。

```text
UIView * view;
...
view.growingAttributesInfo = "The New iPad";
```

#### 忽略某元素 {#忽略某元素}

```text
UIView * view;
...
view.growingAttributesDonotTrack = YES;
```

其中，view是您需要忽略的元素。

#### 自定义数据接收服务器 {#自定义数据接收服务器}

GrowingIO SDK 支持自定义行为数据接收服务器，用户可以选择数据先采集到自己的服务器做一些处理（比如审计）然后转发到 GrowingIO 服务器。详情参见 [CustomTrackerHost](https://github.com/growingio/help_site/tree/f4b4103b288205f6a9b13e0c4692f4d65a2ab386/Features/CustomTrackerHost.html)

#### 在 App Store 提交你的应用 {#在-app-store-提交你的应用}

集成了 GrowingIO SDK 以后，默认会启用 IDFA，所以在向 App Store 提交应用时，需要：

1. 对于问题 **Does this app use the Advertising Identifier \(IDFA\)**，选择 YES。
2. 对于选项 **Attribute this app installation to a previously served advertisement**，打勾。
3. 对于选项 **Attribute an action taken within this app to a previously served advertisement**，打勾。

至此，您的SDK安装就成功了。您登录 GrowingIO 进入产品安装页面执行“数据检测”几分钟后就可以看到数据了。

