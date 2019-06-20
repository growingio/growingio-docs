# iOS SDK 1.X旧版本升级指导

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
* CS11-20字段，会迁移至“[用户变量](https://docs.growingio.com/docs/data-definition/user-variable/)”。两者的区别主要在于：用户变量支持自定义的归因方式。

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

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档](https://docs.growingio.com/docs/data-definition/user-variable/loginuserid#pei-zhi-he-shang-chuan)。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量”](https://docs.growingio.com/docs/data-definition/page-variable)。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

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

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](https://docs.growingio.com/docs/data-definition/page-variable#zi-ding-yi-bian-liang-de-pei-zhi-he-shang-chuan)

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

